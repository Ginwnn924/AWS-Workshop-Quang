---
title: "Tổng quan giao diện và chức năng ứng dụng"
date: 2026-08-11
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---
## Giao diện người dùng

### Streamlit Application

Ứng dụng Streamlit cung cấp trải nghiệm đầy đủ với các trang:

| Trang | Đường dẫn | Chức năng |
| --- | --- | --- |
| Login | `/login` | Đăng nhập bằng Cognito credentials |
| Register | `/register` | Đăng ký tài khoản mới |
| Chatbot | `/chatbot` | Giao diện chat chính, hỏi đáp pháp luật |
| Admin | `/admin` | Dashboard quản trị (chỉ hiển thị cho admins) |

### Chatbot Interface

Giao diện chat cho phép người dùng:
- Nhập câu hỏi pháp luật bằng tiếng Việt
- Xem câu trả lời với trích dẫn nguồn điều luật
- Xem thời gian xử lý từng giai đoạn (timings)
- Đánh giá chất lượng câu trả lời (1-5 sao)
- Xem lại lịch sử hội thoại

### Admin Dashboard

Quản trị viên có thể:
- Xem danh sách người dùng và trạng thái
- Enable/Disable tài khoản
- Quản lý nhóm quyền (thêm/xóa user khỏi groups)
- Upload và quản lý tài liệu pháp luật
- Soft-delete tài liệu (không hiển thị trong search nhưng không mất data)
- Xem thống kê sử dụng hệ thống

### Chainlit Interface

Chainlit cung cấp giao diện chat nhẹ hơn, phù hợp cho demo nhanh:
- Không yêu cầu đăng nhập (chế độ đơn giản)
- Tích hợp trực tiếp với RAG pipeline
- Hiển thị sources inline trong conversation

## Các luồng chức năng chính

### Luồng Hỏi đáp pháp luật

1. User đăng nhập → Nhập câu hỏi
2. Hệ thống embed câu hỏi → Search pgvector → Rerank
3. LLM sinh câu trả lời với citations
4. Hiển thị answer + sources + timings
5. User có thể đánh giá feedback

### Luồng Upload tài liệu

1. Admin/Editor đăng nhập → Chọn file PDF/TXT
2. Frontend request presigned URL từ API
3. Upload trực tiếp lên S3
4. Pipeline tự động: S3 → SQS → Lambda → pgvector
5. Tài liệu mới available cho search sau khi processing hoàn tất

### Luồng Quản lý User

1. Admin đăng nhập → Vào Admin Dashboard
2. Xem danh sách users từ Cognito
3. Thực hiện actions: enable, disable, change group
4. Changes reflect ngay lập tức trong Cognito

## API Endpoints

| Method | Endpoint | Mô tả |
| --- | --- | --- |
| POST | `/ask` | Chat đơn giản (không auth) |
| POST | `/api/chat` | Chat với auth + history |
| GET | `/api/conversations` | Lấy lịch sử hội thoại |
| GET | `/api/admin/users` | Liệt kê users |
| POST | `/api/admin/users/{id}/disable` | Disable user |
| POST | `/api/admin/documents` | Upload document metadata |
| DELETE | `/api/admin/documents/{id}` | Soft-delete document |
| GET | `/health` | Health check |
