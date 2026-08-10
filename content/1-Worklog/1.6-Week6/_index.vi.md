---
title: "Worklog Tuần 6"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6

* Thực hành ML pipeline trên SageMaker và quản lý tài nguyên AWS CLI.
* Debug và hoàn thiện luồng RAG end-to-end.
* Thiết kế observability với CloudWatch, SNS và DynamoDB.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - **Thực hành** ML pipeline trên SageMaker: notebook, training job, model registry <br> - So sánh workflow SageMaker với pipeline build index tự viết | 27/07/2026 | 27/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Quản lý tài nguyên bằng AWS CLI: S3 sync, SNS topic, IAM policy, VPC/EC2 describe <br> - Viết script tự động hóa deploy và health check | 28/07/2026 | 28/07/2026 | [AWS IAM](https://000002.awsstudygroup.com/vi/) <br> [Amazon S3](https://000057.awsstudygroup.com/vi/) |
| 4 | - Debug RAG: điều chỉnh `top_k`, prompt template, chunk size, API timeout <br> - Hoàn thiện luồng ingest → retrieve → generate → citation trên RDS | 29/07/2026 | 29/07/2026 | Source code `rag_core/`, `debug_search.py` |
| 5 | - Thiết kế CloudWatch metric/alarm → SNS email khi latency hoặc error rate cao <br> - Thiết kế DynamoDB table lưu chat history (session_id, timestamp, Q&A) | 30/07/2026 | 30/07/2026 | [Lambda + S3 + DynamoDB](https://000078.awsstudygroup.com/vi/) <br> `docs/IMPLEMENTATION_PLAN.md` |
| 6 | - Hoàn thành Worklog tuần 2–4 và Blog 1 theo deadline FCAJ <br> - Rà soát nội dung blog: mô tả kiến trúc RAG và bài học tuần 2–4 | 31/07/2026 | 31/07/2026 | [FCAJ Kick off](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |

### Kết quả đạt được tuần 6

* Chatbot hỏi đáp ổn định hơn sau tuning và debug.
* Có thiết kế observability và lưu lịch sử hội thoại rõ ràng.
* Hoàn thành Worklog tuần 2–4 và Blog 1 đúng tiến độ.
