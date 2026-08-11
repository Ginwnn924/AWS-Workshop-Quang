---
title: "Deployment Steps"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---
This section guides the deployment of Vietnamese Legal RAG Chatbot on AWS in the correct order.

## Deployment Contents

1. [Foundation Infrastructure (CloudFormation)](5.4.1-foundation-infra/) — provision Cognito, DynamoDB, S3, SQS/DLQ.
2. [Vector Database on RDS](5.4.2-vector-db-rds/) — create RDS PostgreSQL, install pgvector, build index.
3. [RAG Pipeline](5.4.3-rag-pipeline/) — configure embedding, chunking, retriever, and reranker.
4. [API Deployment with Docker](5.4.4-api-deployment/) — build images, Docker Compose, deploy on EC2.
5. [Authentication & Document Ingestion](5.4.5-auth-ingestion/) — Cognito JWT, RBAC, S3→SQS→Lambda pipeline.
6. [Monitoring & Evaluation](5.4.6-monitoring-evaluation/) — CloudWatch, structured logging, offline metrics.
