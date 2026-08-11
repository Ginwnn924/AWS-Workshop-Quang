---
title: "Hạ tầng nền tảng (CloudFormation)"
date: 2026-08-11
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---
## Mục tiêu

Provisioning toàn bộ hạ tầng nền tảng bằng một CloudFormation template duy nhất (`infra/foundation.yaml`), bao gồm: Cognito User Pool, DynamoDB Table, S3 Bucket và SQS Queue với Dead Letter Queue.

## Các bước thực hiện

### 1. Tạo CloudFormation Stack

Truy cập AWS Console → **CloudFormation** → **Create stack** → **With new resources (standard)**.

Chọn **Upload a template file** và tải file `infra/foundation.yaml` từ repository.

### 2. Cấu hình Parameters

| Parameter | Giá trị mẫu | Mô tả |
| --- | --- | --- |
| ProjectName | `legal-rag` | Prefix cho tất cả resource names |
| Environment | `dev` | Môi trường triển khai |
| AdminEmail | `admin@example.com` | Email quản trị viên nhận thông báo |

### 3. Xác nhận và tạo Stack

Tick checkbox **"I acknowledge that AWS CloudFormation might create IAM resources"** và nhấn **Create stack**. Chờ trạng thái chuyển sang `CREATE_COMPLETE`.

### 4. Ghi nhận Output

Sau khi stack tạo thành công, tab **Outputs** cung cấp các giá trị cần thiết cho file `.env`:

| Output Key | Mô tả |
| --- | --- |
| `CognitoUserPoolId` | User Pool ID cho xác thực |
| `CognitoClientId` | App Client ID |
| `DynamoDBTableName` | Tên bảng lưu chat history |
| `S3BucketName` | Bucket cho document storage |
| `SQSQueueUrl` | URL queue cho ingestion pipeline |
| `DLQQueueUrl` | URL Dead Letter Queue |

### 5. Xác minh tài nguyên

```bash
aws cognito-idp describe-user-pool --user-pool-id <UserPoolId> --region ap-southeast-1
aws dynamodb describe-table --table-name <TableName> --region ap-southeast-1
aws s3 ls s3://<BucketName>/
aws sqs get-queue-attributes --queue-url <QueueUrl> --attribute-names All --region ap-southeast-1
```

## Cognito Groups

Template tự động tạo ba nhóm với precedence phân cấp:

| Group | Precedence | Vai trò |
| --- | --- | --- |
| `admins` | 1 | Toàn quyền quản trị |
| `editors` | 2 | Upload và quản lý tài liệu |
| `users` | 3 | Sử dụng chatbot |
