---
title: "Workshop"
date: 2026-08-11
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

## LLMOps on AWS with Vietnamese Legal RAG Chatbot

## Overview

This workshop guides the entire process of building, deploying, and operating the **Vietnamese Legal RAG Chatbot** — a question-answering system for Vietnamese legal documents based on Retrieval-Augmented Generation (RAG) — on Amazon Web Services (AWS). The project applies modern LLMOps architecture with an automated ingestion pipeline, vector database on Amazon RDS PostgreSQL (pgvector), user authentication via Amazon Cognito, and a comprehensive quality evaluation system.

The workshop is divided into four main phases:

* **Foundation Infrastructure**: Set up Cognito User Pool, DynamoDB, S3, SQS, and RDS PostgreSQL with pgvector extension
* **RAG Pipeline**: Build embedding, chunking, vector store, and retrieval with reranking workflow
* **Application Deployment**: Dockerize and deploy FastAPI and Streamlit on EC2 with Docker Compose
* **Operations & Evaluation**: Monitor performance, collect feedback, and evaluate retrieval quality

## Architecture Overview

The system is organized into five main layers:

| **Layer** | **Components** |
| --- | --- |
| Ingestion | Amazon S3, Amazon SQS, AWS Lambda, Text Chunking |
| Embedding & Storage | SentenceTransformers / Bedrock Titan, Amazon RDS PostgreSQL + pgvector |
| Retrieval & Generation | Cosine Similarity Search, Cross-encoder Reranker, Google Gemini / Amazon Bedrock |
| Application | FastAPI, Streamlit, Chainlit, Docker Compose |
| Auth & Monitoring | Amazon Cognito, Amazon DynamoDB, Amazon CloudWatch |

## Contents

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [System Architecture](5.3-Structure/)
4. [Deployment Steps](5.4-Onprem/)
5. [Deployment Verification & System Evaluation](5.5-Policy/)
6. [Application UI & Features Overview](5.6-Project-Visual/)
7. [Clean Up Resources](5.7-Cleanup/)
