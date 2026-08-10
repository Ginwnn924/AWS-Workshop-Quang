---
title: "Worklog Tuần 8"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.8. </b> "
---

### Mục tiêu tuần 8

* Hoàn thiện deploy Docker trên EC2 và tích hợp API Gateway.
* Kiểm thử, rà soát bảo mật và benchmark hiệu năng.
* Hoàn thiện báo cáo thực tập và demo cuối kỳ.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Hoàn thiện Docker deploy trên EC2: FastAPI + Chainlit UI chạy cùng container/network <br> - Gắn IAM Instance Role; chuyển vector store từ SQLite sang RDS production | 10/08/2026 | 10/08/2026 | `deploy/Dockerfile` <br> [Amazon RDS](https://000005.awsstudygroup.com/vi/) |
| 3 | - Tích hợp API Gateway REST: route `/chat`, CORS, stage dev/prod <br> - Token auth (Cognito JWT/API key); hiển thị citation nguồn tài liệu trên UI | 11/08/2026 | 11/08/2026 | [Frontend gọi API Gateway](https://000079.awsstudygroup.com/vi/) <br> [AWS Cognito](https://000081.awsstudygroup.com/vi/) |
| 4 | - Kiểm thử ingestion pipeline với file pháp luật mới; verify chunk trong RDS <br> - Benchmark latency P50/P95 bằng `benchmark_qa.py`; rà soát bảo mật (.env, AUTH_DISABLED, Secrets Manager) | 12/08/2026 | 12/08/2026 | `benchmark_qa.py` <br> [AWS IAM](https://000002.awsstudygroup.com/vi/) |
| 5 | - Sửa lỗi còn lại (timeout, CORS, empty retrieval) <br> - Hoàn thiện báo cáo Hugo, Blog 3, Event 3; deploy site lên GitHub Pages | 13/08/2026 | 13/08/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Demo cuối kỳ dự án Law-Chatbot với mentor: hỏi đáp pháp luật + trích dẫn nguồn <br> - Nộp worklog đầy đủ 8 tuần; dọn dẹp resource AWS không dùng | 14/08/2026 | 14/08/2026 | [AWS Support & Cleanup](https://000009.awsstudygroup.com/vi/) <br> [hcm-rules.awsfcaj.com](https://hcm-rules.awsfcaj.com/) |

### Kết quả đạt được tuần 8

* Prototype Vietnamese Legal Chatbot deploy và demo thành công.
* Báo cáo, blog, event và worklog hoàn chỉnh đúng deadline.
* Nắm luồng RAG + AWS end-to-end trên môi trường thực hành.
