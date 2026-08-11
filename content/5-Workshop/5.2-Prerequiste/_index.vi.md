---
title: "Các bước chuẩn bị"
date: 2026-08-11
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---
## Tài khoản & công cụ

| **Yêu cầu** | **Chi tiết** |
| --- | --- |
| Tài khoản AWS | Tài khoản cá nhân; Region `ap-southeast-1` (Singapore), MFA bật. |
| AWS CLI | AWS CLI v2, đã cấu hình profile/IAM role phù hợp. |
| Python | Python 3.11+ với `pip` và `venv` cho FastAPI backend. |
| Docker | Docker Engine và Docker Compose để build/deploy ứng dụng. |
| Git / GitHub | Repository source code dự án. |
| Google Gemini API Key | API key cho Google Generative AI (Gemini 2.5 Flash). |
| HuggingFace Account | Truy cập dataset pháp luật và model embedding tiếng Việt. |
| PostgreSQL Client | `psql` hoặc DBeaver để kiểm tra kết nối RDS. |
| Tệp hạ tầng | `infra/foundation.yaml` — CloudFormation template. |

## IAM permissions

Gắn IAM permission policy sau vào tài khoản AWS user để triển khai và dọn dẹp tài nguyên trong workshop này.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "CloudFormationManagement",
      "Effect": "Allow",
      "Action": [
        "cloudformation:CreateStack",
        "cloudformation:UpdateStack",
        "cloudformation:DeleteStack",
        "cloudformation:DescribeStacks",
        "cloudformation:DescribeStackEvents",
        "cloudformation:DescribeStackResources",
        "cloudformation:ListStacks",
        "cloudformation:ValidateTemplate"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CognitoManagement",
      "Effect": "Allow",
      "Action": [
        "cognito-idp:CreateUserPool",
        "cognito-idp:DeleteUserPool",
        "cognito-idp:DescribeUserPool",
        "cognito-idp:UpdateUserPool",
        "cognito-idp:CreateUserPoolClient",
        "cognito-idp:DeleteUserPoolClient",
        "cognito-idp:CreateGroup",
        "cognito-idp:DeleteGroup",
        "cognito-idp:AdminCreateUser",
        "cognito-idp:AdminDeleteUser",
        "cognito-idp:AdminAddUserToGroup",
        "cognito-idp:AdminRemoveUserFromGroup",
        "cognito-idp:ListUsers",
        "cognito-idp:ListGroups"
      ],
      "Resource": "*"
    },
    {
      "Sid": "DynamoDBManagement",
      "Effect": "Allow",
      "Action": [
        "dynamodb:CreateTable",
        "dynamodb:DeleteTable",
        "dynamodb:DescribeTable",
        "dynamodb:UpdateTable",
        "dynamodb:PutItem",
        "dynamodb:GetItem",
        "dynamodb:Query",
        "dynamodb:DeleteItem",
        "dynamodb:UpdateTimeToLive"
      ],
      "Resource": "*"
    },
    {
      "Sid": "S3AndSQSManagement",
      "Effect": "Allow",
      "Action": [
        "s3:CreateBucket",
        "s3:DeleteBucket",
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject",
        "s3:ListBucket",
        "s3:PutBucketNotification",
        "s3:PutBucketPolicy",
        "sqs:CreateQueue",
        "sqs:DeleteQueue",
        "sqs:GetQueueAttributes",
        "sqs:SetQueueAttributes",
        "sqs:SendMessage",
        "sqs:ReceiveMessage",
        "sqs:DeleteMessage"
      ],
      "Resource": "*"
    },
    {
      "Sid": "RDSManagement",
      "Effect": "Allow",
      "Action": [
        "rds:CreateDBInstance",
        "rds:DeleteDBInstance",
        "rds:DescribeDBInstances",
        "rds:ModifyDBInstance",
        "rds:StartDBInstance",
        "rds:StopDBInstance",
        "rds:CreateDBSubnetGroup",
        "rds:DeleteDBSubnetGroup",
        "rds:DescribeDBSubnetGroups"
      ],
      "Resource": "*"
    },
    {
      "Sid": "LambdaManagement",
      "Effect": "Allow",
      "Action": [
        "lambda:CreateFunction",
        "lambda:DeleteFunction",
        "lambda:UpdateFunctionCode",
        "lambda:UpdateFunctionConfiguration",
        "lambda:InvokeFunction",
        "lambda:GetFunction",
        "lambda:AddPermission",
        "lambda:CreateEventSourceMapping",
        "lambda:DeleteEventSourceMapping"
      ],
      "Resource": "*"
    },
    {
      "Sid": "EC2AndVPCManagement",
      "Effect": "Allow",
      "Action": [
        "ec2:RunInstances",
        "ec2:TerminateInstances",
        "ec2:DescribeInstances",
        "ec2:CreateVpc",
        "ec2:DeleteVpc",
        "ec2:CreateSubnet",
        "ec2:DeleteSubnet",
        "ec2:CreateSecurityGroup",
        "ec2:DeleteSecurityGroup",
        "ec2:AuthorizeSecurityGroupIngress",
        "ec2:RevokeSecurityGroupIngress",
        "ec2:CreateInternetGateway",
        "ec2:DeleteInternetGateway",
        "ec2:AttachInternetGateway",
        "ec2:DetachInternetGateway"
      ],
      "Resource": "*"
    },
    {
      "Sid": "IAMRolesForLambda",
      "Effect": "Allow",
      "Action": [
        "iam:CreateRole",
        "iam:DeleteRole",
        "iam:AttachRolePolicy",
        "iam:DetachRolePolicy",
        "iam:PutRolePolicy",
        "iam:DeleteRolePolicy",
        "iam:PassRole",
        "iam:GetRole"
      ],
      "Resource": "*"
    }
  ]
}
```

## Khởi tạo hạ tầng nền tảng bằng CloudFormation

Từ AWS Console, vào **CloudFormation** → **Create stack** → **Upload a template file** → tải file `infra/foundation.yaml`. Template này tự động tạo:

- **Amazon Cognito User Pool** với ba nhóm quyền: `users`, `editors`, `admins`
- **Amazon DynamoDB Table** cho lưu trữ lịch sử hội thoại với TTL
- **Amazon S3 Bucket** cho document storage và ingestion
- **Amazon SQS Queue + Dead Letter Queue** cho pipeline xử lý tài liệu

Khi nhập parameters, cung cấp tên dự án (prefix cho resource naming) và email quản trị viên. Xác nhận quyền tạo IAM resources rồi tạo stack. Chờ stack đạt trạng thái `CREATE_COMPLETE`.

## Cấu hình biến môi trường

Tham khảo file `.env.sample` trong repository để thiết lập các biến cấu hình. Các biến quan trọng:

| Nhóm | Biến | Mô tả |
| --- | --- | --- |
| Database | `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD` | Kết nối RDS PostgreSQL |
| LLM | `LLM_PROVIDER`, `GEMINI_API_KEY`, `AWS_REGION` | Provider và API key cho LLM |
| Embedding | `EMBEDDING_MODEL`, `EMBEDDING_DIMENSION` | Model embedding tiếng Việt |
| Auth | `COGNITO_USER_POOL_ID`, `COGNITO_CLIENT_ID`, `COGNITO_REGION` | Cognito authentication |
| Storage | `S3_BUCKET_NAME`, `SQS_QUEUE_URL` | Ingestion pipeline |
| App | `APP_MODE`, `CHUNK_SIZE`, `CHUNK_OVERLAP`, `TOP_K`, `RERANK_ENABLED` | RAG pipeline parameters |

Không commit file `.env` vào repository. Sử dụng `.env.sample` làm template và tạo `.env` local cho mỗi môi trường.
