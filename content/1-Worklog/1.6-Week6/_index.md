---
title: "Week 6 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives

* Practice ML pipeline on SageMaker and manage AWS resources via CLI.
* Debug and complete the end-to-end RAG flow.
* Design observability with CloudWatch, SNS, and DynamoDB.

### Tasks carried out this week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - **Practice** ML pipeline on SageMaker: notebook, training job, model registry <br> - Compare SageMaker workflow with custom build index pipeline | 27/07/2026 | 27/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Manage resources via AWS CLI: S3 sync, SNS topic, IAM policy, VPC/EC2 describe <br> - Write automation scripts for deploy and health check | 28/07/2026 | 28/07/2026 | [AWS IAM](https://000002.awsstudygroup.com/vi/) <br> [Amazon S3](https://000057.awsstudygroup.com/vi/) |
| 4 | - Debug RAG: tune `top_k`, prompt template, chunk size, API timeout <br> - Complete ingest → retrieve → generate → citation flow on RDS | 29/07/2026 | 29/07/2026 | Source code `rag_core/`, `debug_search.py` |
| 5 | - Design CloudWatch metric/alarm → SNS email on high latency or error rate <br> - Design DynamoDB table for chat history (session_id, timestamp, Q&A) | 30/07/2026 | 30/07/2026 | [Lambda + S3 + DynamoDB](https://000078.awsstudygroup.com/vi/) <br> `docs/IMPLEMENTATION_PLAN.md` |
| 6 | - Complete Week 2–4 Worklogs and Blog 1 per FCAJ deadline <br> - Review blog content: RAG architecture and lessons from weeks 2–4 | 31/07/2026 | 31/07/2026 | [FCAJ Kick off](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |

### Week 6 Achievements

* Chatbot Q&A more stable after tuning and debugging.
* Clear observability design and conversation history storage.
* Completed Week 2–4 Worklogs and Blog 1 on schedule.
