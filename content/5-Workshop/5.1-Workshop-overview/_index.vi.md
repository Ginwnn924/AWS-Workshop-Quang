---
title: "Overview"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Overview — Kiến trúc dự án Law-Chatbot

## Giới thiệu dự án

**Law-Chatbot** (Trợ lý Pháp luật Việt Nam) là hệ thống hỏi đáp dựa trên **RAG** trong codebase **vietnamese-legal-llmops**. Mục tiêu là giúp người dùng tra cứu văn bản pháp luật Việt Nam và nhận câu trả lời có **căn cứ điều/khoản**, thay vì chỉ dựa vào kiến thức nội tại của LLM.

**Bài toán:** Corpus luật dài, phân mảnh (Luật, Nghị định, Thông tư…); tìm đúng đoạn liên quan tốn thời gian và dễ bỏ sót ngữ cảnh.

**Giải pháp:** Pipeline end-to-end — nạp corpus → chunk → embed → lưu vector → retrieve top-k → LLM sinh câu trả lời **chỉ từ ngữ cảnh đã retrieve**, kèm nguồn trích dẫn.

## Đối tượng sử dụng

| Đối tượng | Vai trò |
| --- | --- |
| **Người dùng cuối** | Đăng ký / đăng nhập → chat, lịch sử phiên, phản hồi thích/không thích |
| **Admin** | Dashboard KPI, quản lý user, xem log, chỉnh cấu hình RAG trên UI Streamlit |
| **Nhóm Cognito** | users / editors / admins — dùng cho API đầy đủ **/api/***: chat, upload tài liệu, quản trị Cognito |

## Kiến trúc tổng quan

![Kiến trúc Vietnamese Legal RAG Chatbot](/images/2-Proposal/legal_chatbot_architecture.png)

Ở mức cao, hệ thống gồm:

- **Frontend:** Streamlit hoặc Chainlit
- **Backend API:** FastAPI
- **RAG Core:** QAService — retrieve, prompt, generate
- **Storage:** RDS PostgreSQL + pgvector; app DB; tùy chọn S3 / DynamoDB
- **AWS:** EC2 Docker, Bedrock, Lambda/SQS, Cognito, VPC…

**Đường deploy demo hiện tại:** Streamlit cổng **8501** gọi FastAPI **api.main** cổng **8000** qua **POST /ask**, vector và app data trên **RDS Postgres**. Instance demo dùng **EC2 t3a.small**.

- Streamlit :8501 → **POST /ask** → FastAPI api.main :8000 → QAService → RDS pgvector + Bedrock/Gemini
- Chainlit app.py → QAService trực tiếp trong process
- FastAPI api.app **/api/*** → Cognito JWT → QAService + admin services

Chi tiết từng lớp: [Frontend](5.1.1-frontend/) và [Backend](5.1.2-backend/). Các mục **5.3 trở đi** hướng dẫn setup từng dịch vụ AWS để deploy.

## Tech stack

| Lớp | Lựa chọn |
| --- | --- |
| UI | Streamlit, Chainlit |
| API | FastAPI + uvicorn |
| RAG | Python tự viết trong src/rag_core |
| Embedding | Model tiếng Việt local hoặc Bedrock Titan |
| LLM | Gemini hoặc Amazon Bedrock Converse |
| Vector store | PostgreSQL + pgvector, hoặc FAISS + SQLite khi local |
| App DB | Postgres hoặc SQLite |
| Auth UI Streamlit | Username/password trong app DB |
| Auth API đầy đủ | Cognito JWT |
| Deploy | Docker Compose trên EC2 |
| IaC | infra/foundation.yaml, migrations/*.sql |

## Cấu trúc repository

| Path | Vai trò |
| --- | --- |
| streamlit_app.py | Entry Streamlit |
| app.py | Entry Chainlit |
| views/ | login, register, chatbot, admin |
| assets/style.css, .streamlit/config.toml | Giao diện và cổng **8501** |
| .env.sample | Mẫu biến môi trường |
| src/api/ | main.py (**POST /ask**) và app.py + routes (**/api**) |
| src/rag_core/ | chunk, embed, retrieve, prompt, generate, lambda |
| src/storage/ | sqlite_store / postgres_store |
| src/services/ | cognito, ingestion, chat_history, document_admin |
| scripts/ | build_index, run_api, init_app_db, sync_to_s3, eval |
| deploy/ | Dockerfile, docker-compose, entrypoint, README |
| infra/, migrations/, docs/, data_demo/, test/ | IaC, SQL, tài liệu, demo data |

## Ba luồng chính

### 1. Luồng nạp dữ liệu

Corpus được đọc từ HuggingFace hoặc file PDF/TXT do admin upload. Văn bản được cắt chunk, tạo embedding và ghi vào vector store. Trên cloud: admin nhận URL upload S3 → object vào prefix incoming → SQS → Lambda chunk/embed vào RDS pgvector.

### 2. Luồng hỏi đáp

Người dùng gửi câu hỏi qua Streamlit hoặc Chainlit/API. Hệ thống embed câu hỏi, tìm đoạn luật tương đồng trên pgvector/FAISS, ghép prompt có ngữ cảnh, gọi Gemini hoặc Bedrock, trả về câu trả lời kèm sources và điểm số.

### 3. Luồng vận hành

Build index, khởi tạo bảng app DB, deploy hai container trên EC2, tùy chọn gắn Cognito / DynamoDB / S3 / SQS qua CloudFormation. AUTH_DISABLED chỉ dùng cho compose/dev gắn Streamlit — không dùng cho production có Cognito.

## Các phần kiến trúc chi tiết

1. [Frontend](5.1.1-frontend/)
2. [Backend](5.1.2-backend/)
