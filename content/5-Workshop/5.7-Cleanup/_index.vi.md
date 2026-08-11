---
title: "Dọn dẹp tài nguyên"
date: 2026-08-11
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---
## Dọn dẹp tài nguyên

Nhiều dịch vụ AWS tính phí theo thời gian hoạt động hoặc dung lượng lưu trữ dù ứng dụng không có người dùng truy cập. Với Vietnamese Legal RAG Chatbot, các nguồn chi phí cần theo dõi sát gồm Amazon RDS PostgreSQL, EC2 instance, Amazon S3, Amazon DynamoDB và Lambda invocations.

## Tiết kiệm chi phí trong quá trình phát triển

Trong thời gian phát triển, có thể dừng các tài nguyên tính phí theo thời gian chạy sau mỗi phiên làm việc:

| Tài nguyên | Hành động tiết kiệm | Lưu ý |
| --- | --- | --- |
| EC2 instance | Stop instance khi không sử dụng | EBS volume vẫn tính phí; data không mất |
| RDS PostgreSQL | Stop DB instance | Tự động restart sau 7 ngày; storage vẫn tính phí |
| Docker containers | `docker-compose down` trên EC2 | Không tốn compute khi containers stopped |

```bash
# Tạm ngưng cuối ngày
aws ec2 stop-instances --instance-ids <EC2_INSTANCE_ID> --region ap-southeast-1
aws rds stop-db-instance --db-instance-identifier legal-rag-db --region ap-southeast-1

# Khởi động lại trước phiên làm việc
aws rds start-db-instance --db-instance-identifier legal-rag-db --region ap-southeast-1
aws ec2 start-instances --instance-ids <EC2_INSTANCE_ID> --region ap-southeast-1
# SSH vào EC2 và chạy docker-compose up -d
```

## Dọn dẹp toàn bộ

Thứ tự xóa **quan trọng** vì nhiều tài nguyên phụ thuộc lẫn nhau:

| Bước | Tài nguyên | Thao tác và lý do |
| --- | --- | --- |
| 1 | Docker Compose | SSH vào EC2, `docker-compose down -v` để xóa containers và volumes |
| 2 | EC2 instance | Terminate instance (EBS volume sẽ bị xóa nếu `DeleteOnTermination=true`) |
| 3 | RDS PostgreSQL | Xóa DB instance; quyết định có cần final snapshot không |
| 4 | DB Subnet Group | Xóa subnet group sau khi RDS đã bị xóa |
| 5 | CloudFormation Stack | Xóa stack `legal-rag-foundation` (tự động xóa Cognito, DynamoDB, S3, SQS) |
| 6 | S3 Bucket | Làm rỗng bucket trước khi CloudFormation xóa (nếu bucket có data) |
| 7 | Lambda Function | Xóa function và event source mapping |
| 8 | VPC Resources | Xóa Security Groups, Subnets, Internet Gateway, Route Tables, VPC |
| 9 | IAM Roles | Xóa Lambda execution role và policies |
| 10 | CloudWatch Logs | Xóa log groups `/legal-rag/*` |

### Nhóm lệnh dọn dẹp

```bash
REGION="ap-southeast-1"

# 1. Terminate EC2
aws ec2 terminate-instances --instance-ids <EC2_INSTANCE_ID> --region $REGION

# 2. Xóa RDS (không cần final snapshot cho dev)
aws rds delete-db-instance \
  --db-instance-identifier legal-rag-db \
  --skip-final-snapshot \
  --region $REGION

# 3. Chờ RDS xóa xong, sau đó xóa subnet group
aws rds delete-db-subnet-group \
  --db-subnet-group-name legal-rag-db-subnet \
  --region $REGION

# 4. Làm rỗng S3 bucket trước khi xóa stack
aws s3 rm s3://<BUCKET_NAME> --recursive --region $REGION

# 5. Xóa CloudFormation stack (xóa Cognito, DynamoDB, SQS)
aws cloudformation delete-stack \
  --stack-name legal-rag-foundation \
  --region $REGION

# 6. Xóa Lambda
aws lambda delete-function \
  --function-name legal-rag-ingestion \
  --region $REGION

# 7. Xóa VPC resources (security groups, subnets, IGW, VPC)
aws ec2 delete-security-group --group-id <SG_APP_ID> --region $REGION
aws ec2 delete-security-group --group-id <SG_RDS_ID> --region $REGION
aws ec2 detach-internet-gateway --internet-gateway-id <IGW_ID> --vpc-id <VPC_ID> --region $REGION
aws ec2 delete-internet-gateway --internet-gateway-id <IGW_ID> --region $REGION
aws ec2 delete-subnet --subnet-id <PUBLIC_SUBNET_ID> --region $REGION
aws ec2 delete-subnet --subnet-id <PRIVATE_SUBNET_ID> --region $REGION
aws ec2 delete-vpc --vpc-id <VPC_ID> --region $REGION

# 8. Xóa CloudWatch log groups
aws logs delete-log-group --log-group-name /legal-rag/api --region $REGION
aws logs delete-log-group --log-group-name /legal-rag/streamlit --region $REGION
aws logs delete-log-group --log-group-name /legal-rag/lambda-ingestion --region $REGION
```

## Kiểm chứng không còn tài nguyên tính phí

```bash
REGION="ap-southeast-1"

# EC2
aws ec2 describe-instances --region $REGION \
  --filters "Name=tag:Name,Values=*legal-rag*" "Name=instance-state-name,Values=running,stopped" \
  --query "Reservations[].Instances[].InstanceId"

# RDS
aws rds describe-db-instances --region $REGION \
  --query "DBInstances[?DBInstanceIdentifier=='legal-rag-db'].DBInstanceIdentifier"

# CloudFormation
aws cloudformation describe-stacks --region $REGION \
  --stack-name legal-rag-foundation 2>&1 | grep -q "does not exist" && echo "Stack deleted"

# Lambda
aws lambda get-function --function-name legal-rag-ingestion --region $REGION 2>&1 | grep -q "ResourceNotFoundException" && echo "Function deleted"
```
