---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.8. </b> "
---

### Week 8 Objectives

* Complete Docker deployment on EC2 and API Gateway integration.
* Test, review security, and benchmark performance.
* Finalize internship report and end-of-term demo.

### Tasks carried out this week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Complete Docker deploy on EC2: FastAPI + Chainlit UI in shared container/network <br> - Attach IAM Instance Role; migrate vector store from SQLite to production RDS | 10/08/2026 | 10/08/2026 | `deploy/Dockerfile` <br> [Amazon RDS](https://000005.awsstudygroup.com/vi/) |
| 3 | - Integrate API Gateway REST: `/chat` route, CORS, dev/prod stages <br> - Token auth (Cognito JWT/API key); display source citations in UI | 11/08/2026 | 11/08/2026 | [Frontend calls API Gateway](https://000079.awsstudygroup.com/vi/) <br> [AWS Cognito](https://000081.awsstudygroup.com/vi/) |
| 4 | - Test ingestion pipeline with new legal files; verify chunks in RDS <br> - Benchmark P50/P95 latency via `benchmark_qa.py`; security review (.env, AUTH_DISABLED, Secrets Manager) | 12/08/2026 | 12/08/2026 | `benchmark_qa.py` <br> [AWS IAM](https://000002.awsstudygroup.com/vi/) |
| 5 | - Fix remaining issues (timeout, CORS, empty retrieval) <br> - Complete Hugo report, Blog 3, Event 3; deploy site to GitHub Pages | 13/08/2026 | 13/08/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 6 | - End-of-term Law-Chatbot demo with mentor: legal Q&A + source citations <br> - Submit full 8-week worklog; clean up unused AWS resources | 14/08/2026 | 14/08/2026 | [AWS Support & Cleanup](https://000009.awsstudygroup.com/vi/) <br> [hcm-rules.awsfcaj.com](https://hcm-rules.awsfcaj.com/) |

### Week 8 Achievements

* Vietnamese Legal Chatbot prototype deployed and demoed successfully.
* Report, blogs, events, and worklog completed on deadline.
* Mastered end-to-end RAG + AWS flow in a hands-on environment.
