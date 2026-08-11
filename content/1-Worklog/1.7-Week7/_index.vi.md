---
title: "Worklog Tuần 7"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7

* Thực hành S3 static website và CloudFront.
* Tích hợp Cognito, FastAPI backend và pipeline ingestion serverless.
* Hoàn thành Worklog tuần 5–7, Event 1 và Blog 1, 2, 3.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Thực hành lab S3: static website hosting, bucket policy, versioning <br> - Cấu hình CloudFront distribution; cross-region copy cho backup corpus | 03/08/2026 | 03/08/2026 | [Amazon S3](https://000057.awsstudygroup.com/vi/) |
| Thứ 3 | - Tích hợp Cognito User Pool + JWT authorizer; phân quyền RBAC chat/admin <br> - Hoàn thiện FastAPI backend: route chat, admin upload, health check | 04/08/2026 | 04/08/2026 | [AWS Cognito](https://000081.awsstudygroup.com/vi/) <br> Source code `auth.py`, `api/` |
| Thứ 4 | - DynamoDB lưu hội thoại theo session; query lịch sử chat cho admin <br> - Presigned S3 upload URL cho admin upload tài liệu pháp luật mới | 05/08/2026 | 05/08/2026 | [Lambda + S3 + DynamoDB](https://000078.awsstudygroup.com/vi/) |
| Thứ 5 | - Xây dựng ingestion serverless: S3 event → SQS → Lambda <br> - Cấu hình retry/DLQ; deploy `foundation.yaml` qua CloudFormation | 06/08/2026 | 06/08/2026 | `infra/foundation.yaml` <br> [Workshop S3 On-Prem](/vi/5-workshop/5.4-s3-onprem/) |
| Thứ 6 | - Hoàn thành Worklog tuần 5–7, Event 1 và Blogs 1, 2, 3 <br> - Kiểm thử end-to-end: upload → ingest → hỏi đáp → citation | 07/08/2026 | 07/08/2026 | [hcm-rules.awsfcaj.com](https://hcm-rules.awsfcaj.com/) |

### Kết quả đạt được tuần 7

* Cognito auth và ingestion serverless hoạt động end-to-end.
* Hoàn thành lab S3/CloudFront và các mốc báo cáo tuần 5–7.
* Foundation stack deploy thành công qua CloudFormation.
