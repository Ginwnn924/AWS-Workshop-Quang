---
title: "Kiến trúc hệ thống"
date: 2026-08-11
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---
## Thiết kế mạng (VPC)

Hệ thống triển khai trong một Amazon VPC tại Region `ap-southeast-1` với cấu trúc subnet phân tách rõ ràng:

| Subnet | Thành phần | Mục đích |
| --- | --- | --- |
| Public Subnet | EC2 instance (Docker Compose), Internet Gateway | Host ứng dụng, tiếp nhận request từ Internet |
| Private Subnet | RDS PostgreSQL + pgvector | Cô lập database, chỉ cho phép kết nối từ application subnet |

### Security Groups

| Security Group | Inbound Rules | Gắn với |
| --- | --- | --- |
| `sg-legal-app` | TCP 8000 (FastAPI), TCP 8501 (Streamlit) từ `0.0.0.0/0` | EC2 instance |
| `sg-legal-rds` | TCP 5432 từ `sg-legal-app` | RDS PostgreSQL |

## Thiết kế cơ sở dữ liệu

### PostgreSQL + pgvector (Vector Store)

RDS PostgreSQL được cấu hình với pgvector extension để lưu trữ và tìm kiếm vector embeddings. Hai loại index được hỗ trợ:

| Index Type | Đặc điểm | Phù hợp khi |
| --- | --- | --- |
| HNSW | Recall cao, build chậm hơn, memory cao hơn | Dataset ổn định, yêu cầu accuracy cao |
| IVFFlat | Build nhanh, memory thấp hơn | Dataset lớn, thường xuyên cập nhật |

Schema chính:

```sql
-- Vector store cho legal documents
CREATE TABLE legal_chunks (
    id SERIAL PRIMARY KEY,
    document_id INTEGER REFERENCES legal_documents(id),
    content TEXT NOT NULL,
    embedding vector(768),
    metadata JSONB,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX ON legal_chunks USING hnsw (embedding vector_cosine_ops);
```

### PostgreSQL (Application Tables)

```sql
-- Bảng quản lý người dùng và session
CREATE TABLE app_users (...);
CREATE TABLE app_chat_sessions (...);
CREATE TABLE app_chat_messages (...);
CREATE TABLE app_feedback (...);
```

### DynamoDB (Chat History)

| Attribute | Type | Mô tả |
| --- | --- | --- |
| `session_id` | String (Partition Key) | ID phiên hội thoại |
| `timestamp` | Number (Sort Key) | Thời điểm message |
| `role` | String | `user` hoặc `assistant` |
| `content` | String | Nội dung message |
| `ttl` | Number | Unix timestamp cho auto-expiry |

## Cognito User Pool & RBAC

Amazon Cognito quản lý xác thực với ba nhóm quyền phân cấp:

| Group | Quyền hạn |
| --- | --- |
| `admins` | Quản lý user, upload/xóa documents, xem metrics, cấu hình hệ thống |
| `editors` | Upload documents, xem conversation history |
| `users` | Chat với hệ thống, xem lịch sử cá nhân, gửi feedback |

JWT token chứa thông tin group trong claim `cognito:groups`; FastAPI middleware kiểm tra claim này để enforce RBAC trên mỗi endpoint.

## Sơ đồ kết nối dịch vụ

```text
┌─────────────────────────────────────────────────────────┐
│                        VPC                               │
│  ┌──────────────────────┐  ┌─────────────────────────┐  │
│  │    Public Subnet      │  │    Private Subnet        │  │
│  │                        │  │                           │  │
│  │  ┌──────────────────┐ │  │  ┌─────────────────────┐ │  │
│  │  │   EC2 Instance    │ │  │  │  RDS PostgreSQL     │ │  │
│  │  │  ┌─────────────┐  │ │  │  │  + pgvector         │ │  │
│  │  │  │ FastAPI:8000 │  │─┼──┼─▶│  (port 5432)       │ │  │
│  │  │  │ Streamlit:   │  │ │  │  └─────────────────────┘ │  │
│  │  │  │   8501       │  │ │  └─────────────────────────┘  │
│  │  │  └─────────────┘  │ │                                 │
│  │  └──────────────────┘ │                                 │
│  └──────────────────────┘                                 │
└─────────────────────────────────────────────────────────┘
         │                          │
         ▼                          ▼
   ┌───────────┐          ┌──────────────┐
   │  Cognito  │          │  DynamoDB    │
   │  (Auth)   │          │  (History)   │
   └───────────┘          └──────────────┘
         │
         ▼
   ┌───────────────────────────────┐
   │  S3 → SQS → Lambda           │
   │  (Document Ingestion)         │
   └───────────────────────────────┘
```
