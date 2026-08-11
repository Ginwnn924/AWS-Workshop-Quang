---
title: "RAG Pipeline"
date: 2026-08-11
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---
## Objective

Configure and test the full RAG pipeline: query embedding, vector retrieval, reranking, and LLM answer generation.

## Pipeline Architecture

```text
Query → Embedding → pgvector Search (Top-K) → Reranker (Top-N) → Prompt → LLM → Answer
```

## Components

1. **Embedding**: AITeamVN/Vietnamese_Embedding (768-dim) via SentenceTransformers
2. **Retriever**: Cosine similarity search on pgvector, configurable TOP_K
3. **Reranker**: Cross-encoder model for precision improvement
4. **Prompt**: Vietnamese legal expert template with context injection
5. **LLM**: Dual provider — Google Gemini 2.5 Flash or Amazon Bedrock
6. **Response**: Answer with citations and per-stage timing breakdown
