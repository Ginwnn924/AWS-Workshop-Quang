---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2

* Thực hành lab VPC và các dịch vụ mạng AWS cơ bản.
* Hoàn thiện pipeline build index cho corpus pháp luật.
* Chuẩn bị hướng triển khai vector store trên RDS pgvector và Bedrock.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Thực hành** triển khai VPC: subnet public/private, route table, Internet Gateway <br> - SSH vào EC2; tạo NAT Gateway | 29/06/2026 | 29/06/2026 | [Amazon VPC](https://000003.awsstudygroup.com/vi/) |
| 3 | - Dùng Reachability Analyzer kiểm tra kết nối mạng <br> - Thiết lập CloudWatch Monitoring/Alerting <br> - Cấu hình VPN theo lab FCAJ | 30/06/2026 | 30/06/2026 | [Amazon VPC](https://000003.awsstudygroup.com/vi/) <br> [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | - Hoàn thiện pipeline build index: streaming/batch đọc metadata và content <br> - Cấu hình chunking giữ cấu trúc Điều/Khoản; commit định kỳ giảm RAM | 01/07/2026 | 01/07/2026 | Source code `pipeline.py`, `chunking.py` |
| 5 | - Viết `scripts/build_index.py`; chạy test chunking, retriever và pipeline <br> - Kiểm tra vector store SQLite local với câu hỏi pháp luật mẫu | 02/07/2026 | 02/07/2026 | pytest, `debug_search.py` |
| 6 | - Tìm hiểu Amazon RDS PostgreSQL pgvector cho vector search trên cloud <br> - Tìm hiểu Bedrock Titan Embedding cho giai đoạn triển khai AWS | 03/07/2026 | 03/07/2026 | [Amazon RDS](https://000005.awsstudygroup.com/vi/) <br> [Amazon Bedrock Baseline Architecture](https://aws.amazon.com/vi/blogs/architecture/amazon-bedrock-baseline-architecture-in-an-aws-landing-zone/) |

### Kết quả đạt được tuần 2

* Hoàn thành lab VPC, NAT Gateway và giám sát CloudWatch cơ bản.
* Pipeline build index chạy ổn định; retrieval trả về chunk pháp luật có score.
* Nắm hướng migrate vector store từ SQLite local lên RDS pgvector trên AWS.
