---
title: "System Architecture"
date: 2026-08-11
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---
## Network Design (VPC)

The system is deployed within an Amazon VPC in Region `ap-southeast-1` with clearly separated subnets: a public subnet hosting the EC2 instance (Docker Compose application) and a private subnet containing RDS PostgreSQL with pgvector.

## Database Design

### PostgreSQL + pgvector (Vector Store)

RDS PostgreSQL is configured with pgvector extension for storing and searching vector embeddings. Supports HNSW and IVFFlat index types for approximate nearest neighbor search.

### DynamoDB (Chat History)

Stores conversation history with partition key `session_id`, sort key `timestamp`, and TTL for automatic session expiry.

## Cognito User Pool & RBAC

Three-tier authorization: `admins` (full system management), `editors` (document upload), `users` (chat and feedback). JWT tokens carry group claims enforced by FastAPI middleware.
