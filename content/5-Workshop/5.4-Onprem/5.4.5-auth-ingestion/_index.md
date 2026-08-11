---
title: "Authentication & Document Ingestion"
date: 2026-08-11
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---
## Objective

Integrate Amazon Cognito JWT authentication with RBAC, and set up the automated S3 → SQS → Lambda ingestion pipeline.

## Steps

1. Configure Cognito JWT verification in FastAPI middleware
2. Set up endpoint-level RBAC (admins/editors/users)
3. Configure S3 event notification to SQS
4. Deploy Lambda function for document processing (chunk + embed + insert)
5. Configure Dead Letter Queue for failed processing
6. Test end-to-end: upload document → verify vector insertion
