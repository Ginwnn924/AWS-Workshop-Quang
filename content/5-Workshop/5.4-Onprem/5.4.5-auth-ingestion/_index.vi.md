---
title: "Authentication và Document Ingestion"
date: 2026-08-11
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---
## Mục tiêu

Tích hợp Amazon Cognito cho xác thực JWT với RBAC, và thiết lập pipeline ingestion tự động S3 → SQS → Lambda.

## Authentication với Cognito

### JWT Flow

```text
User Login → Cognito Auth → JWT Token (with groups claim)
          → FastAPI Request + Bearer Token
          → Middleware verify JWT → Extract groups → Enforce RBAC
```

### Cấu hình FastAPI Auth Middleware

File `src/api/auth.py` thực hiện:
1. Decode và verify JWT token sử dụng Cognito JWKS endpoint
2. Extract `cognito:groups` claim từ token
3. So sánh với required groups cho mỗi endpoint

### Endpoint Authorization

| Endpoint | Method | Required Group | Mô tả |
| --- | --- | --- | --- |
| `/api/chat` | POST | `users` | Gửi câu hỏi pháp luật |
| `/api/conversations` | GET | `users` | Xem lịch sử hội thoại |
| `/api/admin/users` | GET | `admins` | Liệt kê users |
| `/api/admin/users/{id}/disable` | POST | `admins` | Vô hiệu hóa user |
| `/api/admin/documents` | POST | `editors` | Upload tài liệu mới |
| `/api/admin/documents/{id}` | DELETE | `admins` | Soft-delete tài liệu |

## Document Ingestion Pipeline

### Kiến trúc

```text
Admin/Editor → Presigned URL → S3 Upload
S3 Event Notification → SQS Message → Lambda Consumer
Lambda: Download → Parse → Chunk → Embed → Insert pgvector
(On failure) → Dead Letter Queue → Alert/Retry
```

### 1. Cấu hình S3 Event Notification

Khi file mới được upload vào prefix `documents/`, S3 gửi notification đến SQS queue:

```json
{
  "Events": ["s3:ObjectCreated:*"],
  "Filter": {
    "Key": {"FilterRules": [{"Name": "prefix", "Value": "documents/"}]}
  },
  "QueueArn": "arn:aws:sqs:ap-southeast-1:ACCOUNT:legal-rag-ingestion-queue"
}
```

### 2. Lambda Function

Lambda function (`src/rag_core/lambda_handler.py`) xử lý mỗi SQS message:

1. Parse S3 event từ SQS message body
2. Download file từ S3
3. Extract text (hỗ trợ PDF, TXT)
4. Chunk text với overlapping window (`CHUNK_SIZE=512`, `CHUNK_OVERLAP=50`)
5. Embed chunks bằng model embedding
6. Batch insert vectors vào pgvector

### 3. Dead Letter Queue

Nếu Lambda xử lý thất bại sau `maxReceiveCount=3` lần retry, message được chuyển sang DLQ. Quản trị viên kiểm tra DLQ để điều tra và xử lý thủ công.

### 4. Upload tài liệu qua API

```bash
# Tạo presigned URL
curl -X POST http://localhost:8000/api/admin/documents/upload \
  -H "Authorization: Bearer <admin-token>" \
  -H "Content-Type: application/json" \
  -d '{"filename": "luat-dan-su-2015.pdf", "content_type": "application/pdf"}'

# Upload file qua presigned URL
curl -X PUT "<presigned-url>" \
  -H "Content-Type: application/pdf" \
  --data-binary @luat-dan-su-2015.pdf
```

## Xác minh

```bash
# Kiểm tra Cognito auth
curl -X POST http://localhost:8000/api/chat \
  -H "Authorization: Bearer <valid-jwt>" \
  -H "Content-Type: application/json" \
  -d '{"question": "Quyền thừa kế được quy định ở đâu?"}'

# Kiểm tra SQS messages đã xử lý
aws sqs get-queue-attributes \
  --queue-url <QueueUrl> \
  --attribute-names ApproximateNumberOfMessages \
  --region ap-southeast-1

# Kiểm tra DLQ
aws sqs get-queue-attributes \
  --queue-url <DLQUrl> \
  --attribute-names ApproximateNumberOfMessages \
  --region ap-southeast-1
```
