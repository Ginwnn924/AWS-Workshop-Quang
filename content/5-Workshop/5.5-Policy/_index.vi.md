---
title: "Xác minh triển khai và đánh giá hệ thống"
date: 2026-08-11
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---
## Mục tiêu

Xác minh toàn bộ hệ thống hoạt động đúng sau triển khai, bao gồm kiểm tra kết nối, chức năng và hiệu năng.

## Checklist xác minh

### 1. Infrastructure Verification

| Thành phần | Kiểm tra | Kết quả mong đợi |
| --- | --- | --- |
| RDS PostgreSQL | Kết nối từ EC2, query `SELECT version()` | PostgreSQL 15.x |
| pgvector | `SELECT extversion FROM pg_extension WHERE extname='vector'` | 0.5.x+ |
| Vector data | `SELECT COUNT(*) FROM legal_chunks` | > 0 records |
| Cognito | Đăng nhập test user, nhận JWT token | Token valid |
| DynamoDB | Put/Get item test | Success |
| S3 | Upload test file | Success |
| SQS | Send/Receive message test | Message delivered |

### 2. Application Verification

| Chức năng | Cách kiểm tra | Kết quả mong đợi |
| --- | --- | --- |
| Health check | `GET /health` | `{"status": "healthy"}` |
| Simple query | `POST /ask` với câu hỏi pháp luật | Trả về answer + sources |
| Auth flow | Login → Get token → Authenticated request | 200 OK |
| Admin API | List users, list documents | Data returned |
| Chat history | Gửi nhiều messages trong 1 session | DynamoDB lưu đủ |
| Feedback | Submit rating cho một response | Saved to DB |

### 3. Performance Baseline

Chạy benchmark để xác lập baseline hiệu năng:

```bash
python scripts/benchmark_qa.py --iterations 20
```

| Metric | Target | Ghi chú |
| --- | --- | --- |
| p50 latency | < 3s | Bao gồm embedding + search + rerank + LLM |
| p95 latency | < 5s | Chấp nhận cho user-facing chatbot |
| Error rate | < 1% | Trên 100 queries liên tiếp |

### 4. Retrieval Quality

```bash
python scripts/eval.py --test-set data_demo/test_questions.json
```

| Metric | Target | Mô tả |
| --- | --- | --- |
| Recall@10 | > 0.7 | Ít nhất 70% relevant docs trong top-10 |
| MRR | > 0.5 | Kết quả đúng trung bình nằm trong top-2 |

## Xử lý sự cố thường gặp

| Triệu chứng | Nguyên nhân có thể | Giải pháp |
| --- | --- | --- |
| Connection refused port 5432 | Security group chưa cho phép | Thêm inbound rule từ `sg-legal-app` |
| JWT invalid | Cognito region/pool ID sai | Kiểm tra `.env` COGNITO variables |
| Empty search results | Vector store chưa build | Chạy `scripts/build_index.py` |
| LLM timeout | API key hết quota / network issue | Kiểm tra GEMINI_API_KEY hoặc Bedrock access |
| SQS messages stuck | Lambda permission thiếu | Kiểm tra Lambda execution role |
