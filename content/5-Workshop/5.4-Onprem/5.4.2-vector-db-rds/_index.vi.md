---
title: "Vector Database trên RDS"
date: 2026-08-11
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---
## Mục tiêu

Tạo Amazon RDS PostgreSQL instance trong private subnet, cài đặt pgvector extension và xây dựng vector index cho dữ liệu pháp luật.

## Các bước thực hiện

### 1. Tạo VPC và Subnet

Tạo VPC với CIDR `10.0.0.0/16`, bao gồm:
- Public subnet `10.0.1.0/24` cho EC2 application
- Private subnet `10.0.2.0/24` cho RDS database

Tạo Internet Gateway gắn vào VPC, route table cho public subnet trỏ `0.0.0.0/0` qua IGW.

### 2. Tạo DB Subnet Group

```bash
aws rds create-db-subnet-group \
  --db-subnet-group-name legal-rag-db-subnet \
  --db-subnet-group-description "Subnet group for Legal RAG RDS" \
  --subnet-ids subnet-private-1 subnet-private-2 \
  --region ap-southeast-1
```

### 3. Tạo RDS PostgreSQL Instance

| Cấu hình | Giá trị |
| --- | --- |
| Engine | PostgreSQL 15+ |
| Instance class | `db.t3.micro` (dev) / `db.t3.medium` (prod) |
| Storage | 20 GB gp3 |
| DB name | `legal_rag_db` |
| Master username | `postgres` |
| Subnet group | `legal-rag-db-subnet` |
| Security group | `sg-legal-rds` (inbound 5432 from `sg-legal-app`) |
| Public access | No |

### 4. Cài đặt pgvector Extension

Kết nối vào RDS qua EC2 bastion hoặc SSH tunnel:

```sql
CREATE EXTENSION IF NOT EXISTS vector;

-- Tạo bảng legal_documents
CREATE TABLE legal_documents (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    source TEXT,
    file_path TEXT,
    status VARCHAR(20) DEFAULT 'active',
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Tạo bảng legal_chunks với vector column
CREATE TABLE legal_chunks (
    id SERIAL PRIMARY KEY,
    document_id INTEGER REFERENCES legal_documents(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    embedding vector(768),
    chunk_index INTEGER,
    metadata JSONB DEFAULT '{}',
    created_at TIMESTAMP DEFAULT NOW()
);
```

### 5. Tạo Vector Index

Tùy thuộc vào kích thước dataset và yêu cầu hiệu năng, chọn một trong hai loại index:

**HNSW Index** (khuyến nghị cho dataset < 1M vectors):

```sql
CREATE INDEX idx_legal_chunks_embedding_hnsw
ON legal_chunks USING hnsw (embedding vector_cosine_ops)
WITH (m = 16, ef_construction = 64);
```

**IVFFlat Index** (cho dataset lớn hơn):

```sql
CREATE INDEX idx_legal_chunks_embedding_ivfflat
ON legal_chunks USING ivfflat (embedding vector_cosine_ops)
WITH (lists = 100);
```

### 6. Build Vector Store từ Dataset

Chạy script để tải dataset pháp luật từ HuggingFace, chunking và embedding:

```bash
python scripts/build_index.py \
  --dataset "path/to/legal/dataset" \
  --chunk-size 512 \
  --chunk-overlap 50 \
  --batch-size 32
```

Script thực hiện:
1. Đọc dataset (HuggingFace hoặc local parquet)
2. Chia văn bản thành chunks với overlapping window
3. Embed từng batch bằng SentenceTransformer
4. Insert vectors vào pgvector table

### 7. Xác minh

```sql
SELECT COUNT(*) FROM legal_chunks;
SELECT COUNT(*) FROM legal_documents;

-- Test similarity search
SELECT content, 1 - (embedding <=> '[0.1, 0.2, ...]'::vector) AS similarity
FROM legal_chunks
ORDER BY embedding <=> '[0.1, 0.2, ...]'::vector
LIMIT 5;
```
