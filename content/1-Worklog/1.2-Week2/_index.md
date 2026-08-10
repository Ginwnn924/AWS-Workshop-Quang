---
title: "Week 2 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives

* Practice VPC lab and basic AWS networking services.
* Complete the build index pipeline for the legal corpus.
* Prepare RDS pgvector and Bedrock for cloud deployment.

### Tasks carried out this week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - **Practice** VPC deployment: public/private subnets, route tables, Internet Gateway <br> - SSH to EC2; create NAT Gateway | 29/06/2026 | 29/06/2026 | [Amazon VPC](https://000003.awsstudygroup.com/vi/) |
| 3 | - Use Reachability Analyzer to verify network paths <br> - Set up CloudWatch Monitoring/Alerting <br> - Configure VPN per FCAJ lab | 30/06/2026 | 30/06/2026 | [Amazon VPC](https://000003.awsstudygroup.com/vi/) <br> [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| 4 | - Complete build index pipeline: streaming/batch metadata and content reads <br> - Configure chunking to preserve Article/Clause structure; periodic commits to reduce RAM | 01/07/2026 | 01/07/2026 | Source code `pipeline.py`, `chunking.py` |
| 5 | - Write `scripts/build_index.py`; run chunking, retriever, and pipeline tests <br> - Verify local SQLite vector store with sample legal questions | 02/07/2026 | 02/07/2026 | pytest, `debug_search.py` |
| 6 | - Learn Amazon RDS PostgreSQL pgvector for cloud vector search <br> - Learn Bedrock Titan Embedding for AWS deployment phase | 03/07/2026 | 03/07/2026 | [Amazon RDS](https://000005.awsstudygroup.com/vi/) <br> [Amazon Bedrock Baseline Architecture](https://aws.amazon.com/vi/blogs/architecture/amazon-bedrock-baseline-architecture-in-an-aws-landing-zone/) |

### Week 2 Achievements

* Completed VPC, NAT Gateway, and basic CloudWatch monitoring labs.
* Build index pipeline runs stably; retrieval returns scored legal chunks.
* Understood migration path from local SQLite to RDS pgvector on AWS.
