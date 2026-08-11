---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives

* Practice S3 static website and CloudFront labs.
* Integrate Cognito, FastAPI backend, and serverless ingestion pipeline.
* Complete Week 5–7 Worklogs, Event 1, and Blogs 1, 2, 3.

### Tasks carried out this week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Monday | - Practice S3 lab: static website hosting, bucket policy, versioning <br> - Configure CloudFront distribution; cross-region copy for corpus backup | 03/08/2026 | 03/08/2026 | [Amazon S3](https://000057.awsstudygroup.com/vi/) |
| Tuesday | - Practice integrate Cognito User Pool + JWT authorizer; RBAC for chat/admin roles <br> - Complete FastAPI backend: chat, admin upload, health check routes | 04/08/2026 | 04/08/2026 | [AWS Cognito](https://000081.awsstudygroup.com/vi/) <br> Source code auth.py, api/ |
| Wednesday | - DynamoDB stores conversations by session; query chat history for admin <br> - Presigned S3 upload URL for admin to upload new legal documents | 05/08/2026 | 05/08/2026 | [Lambda + S3 + DynamoDB](https://000078.awsstudygroup.com/vi/) |
| Thursday | - Build serverless ingestion: S3 event → SQS → Lambda <br> - Configure retry/DLQ; deploy foundation.yaml via CloudFormation | 06/08/2026 | 06/08/2026 | infra/foundation.yaml <br> [Workshop S3 upload](/5-workshop/5.3-s3-upload/) |
| Friday | - Complete Week 5–7 Worklogs, Event 2, and Blogs 1, 2, 3 <br> - End-to-end test: upload → ingest → Q&A → citation | 07/08/2026 | 07/08/2026 | [hcm-rules.awsfcaj.com](https://hcm-rules.awsfcaj.com/) |

### Week 7 Achievements

* Cognito auth and serverless ingestion work end-to-end.
* Completed S3/CloudFront lab and Week 5–7 reporting milestones.
* Foundation stack deployed successfully via CloudFormation.
