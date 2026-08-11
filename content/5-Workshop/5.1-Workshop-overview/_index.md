---
title: "Overview"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Overview — Law-Chatbot project architecture

## Project introduction

**Law-Chatbot** (Vietnamese Legal Assistant) is a **RAG** Q&A system in the **vietnamese-legal-llmops** codebase. It helps users look up Vietnamese legal documents and get answers **grounded in article/clause citations**, instead of relying only on the LLM’s internal knowledge.

**Problem:** Legal corpora are long and fragmented (laws, decrees, circulars…); finding the right passages is slow and easy to miss context.

**Solution:** An end-to-end pipeline — ingest corpus → chunk → embed → store vectors → retrieve top-k → LLM generates an answer **only from retrieved context**, with cited sources.

## Users

| Audience | Role |
| --- | --- |
| **End user** | Register / login → chat, session history, like/dislike feedback |
| **Admin** | KPI dashboard, user management, logs, RAG settings in the Streamlit UI |
| **Cognito groups** | users / editors / admins — for the full **/api/***: chat, document upload, Cognito admin |

## High-level architecture

![Vietnamese Legal RAG Chatbot architecture](/images/2-Proposal/legal_chatbot_architecture.png)

At a high level the system includes:

- **Frontend:** Streamlit or Chainlit
- **Backend API:** FastAPI
- **RAG Core:** QAService — retrieve, prompt, generate
- **Storage:** RDS PostgreSQL + pgvector; app DB; optional S3 / DynamoDB
- **AWS:** EC2 Docker, Bedrock, Lambda/SQS, Cognito, VPC…

**Current demo deploy path:** Streamlit on port **8501** calls FastAPI **api.main** on port **8000** via **POST /ask**, with vectors and app data on **RDS Postgres**. The demo instance is **EC2 t3a.small**.

- Streamlit :8501 → **POST /ask** → FastAPI api.main :8000 → QAService → RDS pgvector + Bedrock/Gemini
- Chainlit app.py → QAService in-process
- FastAPI api.app **/api/*** → Cognito JWT → QAService + admin services

Layer details: [Frontend](5.1.1-frontend/) and [Backend](5.1.2-backend/). Sections **5.3 onward** cover setting up each AWS service for deployment.

## Tech stack

| Layer | Choices |
| --- | --- |
| UI | Streamlit, Chainlit |
| API | FastAPI + uvicorn |
| RAG | Custom Python in src/rag_core |
| Embedding | Local Vietnamese model or Bedrock Titan |
| LLM | Gemini or Amazon Bedrock Converse |
| Vector store | PostgreSQL + pgvector, or FAISS + SQLite locally |
| App DB | Postgres or SQLite |
| Streamlit auth | Username/password in app DB |
| Full API auth | Cognito JWT |
| Deploy | Docker Compose on EC2 |
| IaC | infra/foundation.yaml, migrations/*.sql |

## Repository layout

| Path | Role |
| --- | --- |
| streamlit_app.py | Streamlit entry |
| app.py | Chainlit entry |
| views/ | login, register, chatbot, admin |
| assets/style.css, .streamlit/config.toml | UI styling and port **8501** |
| .env.sample | Environment variable template |
| src/api/ | main.py (**POST /ask**) and app.py + routes (**/api**) |
| src/rag_core/ | chunk, embed, retrieve, prompt, generate, lambda |
| src/storage/ | sqlite_store / postgres_store |
| src/services/ | cognito, ingestion, chat_history, document_admin |
| scripts/ | build_index, run_api, init_app_db, sync_to_s3, eval |
| deploy/ | Dockerfile, docker-compose, entrypoint, README |
| infra/, migrations/, docs/, data_demo/, test/ | IaC, SQL, docs, demo data |

## Three main flows

### 1. Ingestion flow

Corpus is read from HuggingFace or admin-uploaded PDF/TXT. Text is chunked, embedded, and written to the vector store. On AWS: admin gets an S3 upload URL → object under an incoming prefix → SQS → Lambda chunks/embeds into RDS pgvector.

### 2. RAG query flow

The user asks via Streamlit or Chainlit/API. The system embeds the question, finds similar legal passages in pgvector/FAISS, builds a grounded prompt, calls Gemini or Bedrock, and returns an answer with sources and scores.

### 3. Operations flow

Build the index, initialize app DB tables, deploy two containers on EC2, and optionally wire Cognito / DynamoDB / S3 / SQS via CloudFormation. AUTH_DISABLED is only for Streamlit compose/dev — not for production Cognito deployments.

## Detailed architecture sections

1. [Frontend](5.1.1-frontend/)
2. [Backend](5.1.2-backend/)
