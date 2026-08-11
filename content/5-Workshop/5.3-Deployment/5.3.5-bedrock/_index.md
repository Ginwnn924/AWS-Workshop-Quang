---
title: "Bedrock — Embedding & LLM"
date: 2026-08-11
weight: 5
chapter: false
pre: " <b> 5.3.5. </b> "
---

# Bedrock — Embedding & LLM

Amazon Bedrock provides managed embedding and LLM for the RAG pipeline on AWS — replacing local models or Gemini when deploying to cloud.

{{< mermaid >}}
graph LR
    T["Text / Question"] --> E["Bedrock Embedding"]
    E --> V["Vector"]
    V --> R["pgvector search"]
    R --> C["Retrieved chunks"]
    C --> L["Bedrock LLM"]
    L --> A["Answer"]
{{< /mermaid >}}

| Role | When used | Example model |
| --- | --- | --- |
| **Embedding** | Build index, embed queries | `amazon.titan-embed-text-v2:0` |
| **LLM** | Generate answers from prompt | Claude / Titan via Converse API |

## Steps

1. Enable model access in Bedrock console
2. Configure `.env` with provider and model IDs
3. Attach IAM policy (`bedrock:InvokeModel`) to EC2/Lambda role
4. Test embedding and LLM generation

Refer to the Vietnamese version for full details.
