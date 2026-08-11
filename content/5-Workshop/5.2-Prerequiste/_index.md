---
title: "Prerequisites"
date: 2026-08-11
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---
## Accounts & Tools

| **Requirement** | **Details** |
| --- | --- |
| AWS Account | Personal account; Region `ap-southeast-1` (Singapore), MFA enabled. |
| AWS CLI | AWS CLI v2, configured with appropriate profile/IAM role. |
| Python | Python 3.11+ with `pip` and `venv` for FastAPI backend. |
| Docker | Docker Engine and Docker Compose for build/deploy. |
| Git / GitHub | Project source code repository. |
| Google Gemini API Key | API key for Google Generative AI (Gemini 2.5 Flash). |
| HuggingFace Account | Access to legal datasets and Vietnamese embedding models. |
| PostgreSQL Client | `psql` or DBeaver for RDS connectivity testing. |
| Infrastructure file | `infra/foundation.yaml` — CloudFormation template. |

## Foundation Infrastructure with CloudFormation

From AWS Console, navigate to **CloudFormation** → **Create stack** → **Upload a template file** → upload `infra/foundation.yaml`. This template provisions:

- **Amazon Cognito User Pool** with three groups: `users`, `editors`, `admins`
- **Amazon DynamoDB Table** for conversation history with TTL
- **Amazon S3 Bucket** for document storage and ingestion
- **Amazon SQS Queue + Dead Letter Queue** for document processing pipeline

## Environment Configuration

Refer to `.env.sample` in the repository. Key variables include database connection, LLM provider settings, embedding model configuration, Cognito auth parameters, and RAG pipeline tuning (chunk size, overlap, top-k, rerank toggle).
