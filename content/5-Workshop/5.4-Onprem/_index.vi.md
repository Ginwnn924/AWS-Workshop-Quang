---
title: "Các bước triển khai"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---
Phần này hướng dẫn triển khai Vietnamese Legal RAG Chatbot trên AWS theo đúng thứ tự: hạ tầng nền tảng (CloudFormation), cơ sở dữ liệu vector, RAG pipeline, API deployment, xác thực và ingestion, giám sát và đánh giá.

## Nội dung triển khai

1. [Hạ tầng nền tảng (CloudFormation)](5.4.1-foundation-infra/) — provisioning Cognito, DynamoDB, S3, SQS/DLQ bằng CloudFormation template.
2. [Vector Database trên RDS](5.4.2-vector-db-rds/) — tạo RDS PostgreSQL, cài đặt pgvector extension, tạo schema và build vector index.
3. [RAG Pipeline](5.4.3-rag-pipeline/) — cấu hình embedding model, chunking strategy, retriever và reranker.
4. [API Deployment với Docker](5.4.4-api-deployment/) — build Docker image, thiết lập Docker Compose và triển khai trên EC2.
5. [Authentication và Document Ingestion](5.4.5-auth-ingestion/) — tích hợp Cognito JWT, RBAC và pipeline S3→SQS→Lambda.
6. [Monitoring và Evaluation](5.4.6-monitoring-evaluation/) — cấu hình CloudWatch logs, structured logging, offline evaluation metrics.
