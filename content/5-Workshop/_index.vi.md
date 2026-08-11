---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop: Vietnamese Legal RAG Chatbot

#### Tổng quan

Workshop này trình bày dự án **Law-Chatbot** — hệ thống hỏi đáp pháp luật Việt Nam theo kiến trúc **RAG** trên AWS, dựa trên codebase **vietnamese-legal-llmops**.

Em sẽ đi từ hiểu kiến trúc hệ thống đến các bước chuẩn bị, rồi lần lượt setup từng dịch vụ AWS để deploy dự án.

**Demo:** [http://18.143.187.153:8501/](http://18.143.187.153:8501/)

#### Nội dung

1. [Overview — Kiến trúc dự án](5.1-Workshop-overview/)
   - [5.1.1 Frontend](5.1-Workshop-overview/5.1.1-frontend/)
   - [5.1.2 Backend](5.1-Workshop-overview/5.1.2-backend/)
2. [Prerequisites — Chuẩn bị](5.2-Prerequisite/)
3. [S3 — Tạo bucket và upload data](5.3-s3-upload/)
4. [RDS — PostgreSQL + pgvector](5.4-rds-pgvector/)
5. [Lambda — Ingestion](5.5-lambda/)
6. [Bedrock — Embedding và LLM](5.6-bedrock/)
7. [DynamoDB — Chat history](5.7-dynamodb/)
8. [Cognito — Auth và RBAC](5.8-cognito/)
9. [VPC — Network](5.9-vpc/)
10. [EC2 — Deploy Docker](5.10-ec2-deploy/)
