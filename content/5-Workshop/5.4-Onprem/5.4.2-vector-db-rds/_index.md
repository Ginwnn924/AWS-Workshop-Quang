---
title: "Vector Database on RDS"
date: 2026-08-11
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---
## Objective

Create Amazon RDS PostgreSQL in a private subnet, install pgvector extension, create schema and build vector index for legal data.

## Steps

1. Create VPC with public/private subnets
2. Create DB Subnet Group
3. Provision RDS PostgreSQL instance (PostgreSQL 15+, pgvector-compatible)
4. Install pgvector extension and create tables (`legal_documents`, `legal_chunks`)
5. Create HNSW or IVFFlat vector index
6. Build vector store from legal dataset using `scripts/build_index.py`
7. Verify with similarity search query
