---
title: "Worklog Tuần 5"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5

* Thực hành lab RDS, CloudFormation và CloudWatch.
* Chuyển vector store sang RDS PostgreSQL pgvector.
* Hoàn thành Worklog tuần 1.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Thực hành lab RDS PostgreSQL: tạo instance, subnet group, security group <br> - Lab CloudFormation stack cơ bản; CloudWatch dashboard và alarm <br> - Truy cập S3 từ EC2 bằng IAM Role | 20/07/2026 | 20/07/2026 | [Amazon RDS](https://000005.awsstudygroup.com/vi/) <br> [Amazon S3](https://000057.awsstudygroup.com/vi/) |
| Thứ 3 | - Làm quen SageMaker Studio; xử lý lỗi Service Quotas khi tạo resource <br> - Tìm hiểu embedding model trên Bedrock cho giai đoạn migrate | 21/07/2026 | 21/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [Amazon Bedrock Baseline Architecture](https://aws.amazon.com/vi/blogs/architecture/amazon-bedrock-baseline-architecture-in-an-aws-landing-zone/) |
| Thứ 4 | - Phân tích dataset pháp luật: lọc trùng, chuẩn hóa metadata điều/khoản <br> - Migrate vector store từ SQLite local sang RDS pgvector; chạy lại build index | 22/07/2026 | 22/07/2026 | Source code vector_store/, [Amazon RDS](https://000005.awsstudygroup.com/vi/) |
| Thứ 5 | - Tạo bảng legal_chunks với cột embedding vector(1536) <br> - Tối ưu cosine search; cấu hình connection pool và timeout | 23/07/2026 | 23/07/2026 | pgvector docs, retriever.py |
| Thứ 6 | - Hoàn thành Worklog tuần 1<br> - Rà soát nội dung báo cáo, chỉnh sửa lỗi chính tả và format Hugo | 24/07/2026 | 24/07/2026 | [FCAJ Kick off](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |

### Kết quả đạt được tuần 5

* Retrieval chạy ổn định trên RDS PostgreSQL pgvector.
* Nắm CloudFormation, IAM Role và lab RDS full stack.
* Nộp đúng hạn Worklog tuần 1 và Event 1.
