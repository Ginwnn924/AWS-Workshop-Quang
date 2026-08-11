---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4

* Thực hành lab bảo mật AWS và deploy ứng dụng lên EC2.
* Docker hóa FastAPI/Chainlit cho môi trường triển khai.
* Thiết kế kiến trúc VPC và VPC Endpoint cho dự án.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Thực hành lab IAM: user, group, policy; bật MFA và least privilege <br> - Lab Cognito User Pool; quét Security Hub cơ bản <br> - Khởi tạo EC2 Linux, gắn EBS Snapshot, tạo Custom AMI | 13/07/2026 | 13/07/2026 | [AWS IAM](https://000002.awsstudygroup.com/vi/) <br> [AWS Cognito](https://000081.awsstudygroup.com/vi/) |
| Thứ 3 | - Deploy app Node.js trên EC2 theo lab FCAJ <br> - Cấu hình IAM Instance Profile cho EC2 truy cập AWS service | 14/07/2026 | 14/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| Thứ 4 | - Xây dựng RESTful API FastAPI: endpoint **/chat**, **/health**, CORS <br> - Deploy FastAPI lên EC2; kiểm tra kết nối RDS pgvector từ private subnet | 15/07/2026 | 15/07/2026 | FastAPI docs, source code api/ |
| Thứ 5 | - Viết Dockerfile multi-stage cho FastAPI + Chainlit UI <br> - Build/push image; chạy container trên EC2 với biến môi trường .env | 16/07/2026 | 16/07/2026 | deploy/Dockerfile, Docker docs |
| Thứ 6 | - Thiết kế VPC 3 tầng: public, private, isolated <br> - Cấu hình Gateway Endpoint và Interface Endpoint | 17/07/2026 | 17/07/2026 | [Amazon VPC](https://000003.awsstudygroup.com/vi/) <br> [Workshop Overview](/vi/5-workshop/5.1-workshop-overview/) |

### Kết quả đạt được tuần 4

* Hoàn thành lab bảo mật IAM/Cognito và deploy ứng dụng trên EC2 Linux.
* FastAPI và Docker image sẵn sàng cho triển khai cloud.
* Kiến trúc VPC 3 tầng và VPC Endpoint được thống nhất trong nhóm.
