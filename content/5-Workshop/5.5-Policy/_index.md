---
title: "Deployment Verification & System Evaluation"
date: 2026-08-11
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---
## Objective

Verify the entire system works correctly after deployment, including connectivity, functionality, and performance checks.

## Verification Checklist

1. **Infrastructure**: RDS connectivity, pgvector extension, vector data count, Cognito auth, DynamoDB, S3, SQS
2. **Application**: Health check, query endpoint, auth flow, admin APIs, chat history, feedback
3. **Performance**: p50 < 3s, p95 < 5s, error rate < 1%
4. **Retrieval Quality**: Recall@10 > 0.7, MRR > 0.5
