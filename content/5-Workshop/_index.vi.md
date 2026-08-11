---
title: "Workshop"
date: 2026-08-11
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

## LLMOps trên AWS với Vietnamese Legal RAG Chatbot

## Tổng quan

Workshop này trình bày toàn bộ quy trình xây dựng, triển khai và vận hành **Vietnamese Legal RAG Chatbot** — hệ thống hỏi đáp pháp luật Việt Nam dựa trên mô hình Retrieval-Augmented Generation (RAG) — trên nền tảng Amazon Web Services (AWS). Dự án áp dụng kiến trúc LLMOps hiện đại với pipeline ingestion tự động, vector database trên Amazon RDS PostgreSQL (pgvector), xác thực người dùng qua Amazon Cognito và hệ thống đánh giá chất lượng toàn diện.

Workshop được chia thành bốn giai đoạn chính:

* **Hạ tầng nền tảng**: Thiết lập Cognito User Pool, DynamoDB, S3, SQS và RDS PostgreSQL với pgvector extension
* **Pipeline RAG**: Xây dựng quy trình embedding, chunking, vector store và retrieval với reranking
* **Triển khai ứng dụng**: Đóng gói Docker, triển khai FastAPI và Streamlit trên EC2 với Docker Compose
* **Vận hành và đánh giá**: Giám sát hiệu năng, thu thập feedback và đánh giá chất lượng retrieval

## Tổng quan kiến trúc

Hệ thống được tổ chức theo năm lớp chính:

| **Lớp** | **Thành phần** |
| --- | --- |
| Ingestion | Amazon S3, Amazon SQS, AWS Lambda, Text Chunking |
| Embedding & Storage | SentenceTransformers / Bedrock Titan, Amazon RDS PostgreSQL + pgvector |
| Retrieval & Generation | Cosine Similarity Search, Cross-encoder Reranker, Google Gemini / Amazon Bedrock |
| Application | FastAPI, Streamlit, Chainlit, Docker Compose |
| Auth & Monitoring | Amazon Cognito, Amazon DynamoDB, Amazon CloudWatch |

## Nội dung

1. [Tổng quan workshop](5.1-Workshop-overview/)
2. [Các bước chuẩn bị](5.2-Prerequiste/)
3. [Kiến trúc hệ thống](5.3-Structure/)
4. [Các bước triển khai](5.4-Onprem/)
5. [Xác minh triển khai và đánh giá hệ thống](5.5-Policy/)
6. [Tổng quan giao diện và chức năng ứng dụng](5.6-Project-Visual/)
7. [Dọn dẹp tài nguyên](5.7-Cleanup/)
