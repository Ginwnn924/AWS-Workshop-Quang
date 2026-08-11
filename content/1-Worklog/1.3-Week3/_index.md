---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives

* Finalize the project topic and complete the Law-Chatbot Proposal.
* Complete the RAG flow and Chainlit chat interface.
* Test Q&A and refine prompts for legal source citations.

### Tasks carried out this week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Monday | - Team meeting to finalize Vietnamese Legal Chatbot topic <br> - Write Proposal: goals, scope, use cases, roadmap, and planned AWS services | 06/07/2026 | 06/07/2026 | [Amazon Bedrock Baseline Architecture](https://aws.amazon.com/vi/blogs/architecture/amazon-bedrock-baseline-architecture-in-an-aws-landing-zone/) |
| Tuesday | - Draw Serverless RAG architecture <br> - Update `AWS_chatbot.txt` describing three main flows | 07/07/2026 | 07/07/2026 | [Draw.io Guide](https://www.youtube.com/watch?v=l8isyDe-GwY&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=2) |
| Wednesday | - Complete `retriever`, `prompt`, `generator`, `qa_service` modules <br> - Configure LLM provider: Gemini / Bedrock | 08/07/2026 | 08/07/2026 | Source code `rag_core/` |
| Thursday | - Integrate Chainlit chat UI; stream answers in real time <br> - Design prompts requiring legal article/clause citations | 09/07/2026 | 09/07/2026 | Chainlit docs, `prompt.py` |
| Friday | - Fix Git submodule, IAM, GitHub for team repo <br> - Test Q&A; tune prompt and top_k based on retrieval results | 10/07/2026 | 10/07/2026 | Project GitHub repo <br> [AWS IAM](https://000002.awsstudygroup.com/vi/) |

### Week 3 Achievements

* Completed Proposal and Serverless RAG architecture diagram.
* End-to-end RAG flow works with Chainlit; answers include source chunks.
* Stable GitHub repo; team aligned on cloud architecture for Law-Chatbot.
