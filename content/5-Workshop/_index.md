---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop: Vietnamese Legal RAG Chatbot

#### Overview

This workshop presents the **Law-Chatbot** project — a Vietnamese legal Q&A system using a **RAG** architecture on AWS, based on the **vietnamese-legal-llmops** codebase.

You will start with the system architecture and prerequisites, then set up each AWS service to deploy the project.

**Demo:** [http://18.143.187.153:8501/](http://18.143.187.153:8501/)

#### Contents

1. [Overview — Project architecture](5.1-Workshop-overview/)
   - [5.1.1 Frontend](5.1-Workshop-overview/5.1.1-frontend/)
   - [5.1.2 Backend](5.1-Workshop-overview/5.1.2-backend/)
2. [Prerequisites](5.2-Prerequisite/)
3. [S3 — Create bucket and upload data](5.3-s3-upload/)
4. [RDS — PostgreSQL + pgvector](5.4-rds-pgvector/)
5. [Lambda — Ingestion](5.5-lambda/)
6. [Bedrock — Embedding and LLM](5.6-bedrock/)
7. [DynamoDB — Chat history](5.7-dynamodb/)
8. [Cognito — Auth and RBAC](5.8-cognito/)
9. [VPC — Network](5.9-vpc/)
10. [EC2 — Docker deploy](5.10-ec2-deploy/)
