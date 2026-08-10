---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives

* Practice RDS, CloudFormation, and CloudWatch labs.
* Migrate vector store to RDS PostgreSQL pgvector.
* Complete Week 1 Worklog and Event 1.

### Tasks carried out this week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - **Practice** RDS PostgreSQL lab: create instance, subnet group, security group <br> - Basic CloudFormation stack lab; CloudWatch dashboard and alarms <br> - Access S3 from EC2 via IAM Role (no Access Key on instance) | 20/07/2026 | 20/07/2026 | [Amazon RDS](https://000005.awsstudygroup.com/vi/) <br> [Amazon S3](https://000057.awsstudygroup.com/vi/) |
| 3 | - Explore SageMaker Studio; handle Service Quotas errors when creating resources <br> - Learn Bedrock embedding models for migration phase | 21/07/2026 | 21/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) <br> [Amazon Bedrock Baseline Architecture](https://aws.amazon.com/vi/blogs/architecture/amazon-bedrock-baseline-architecture-in-an-aws-landing-zone/) |
| 4 | - Analyze legal dataset: deduplicate, normalize article/clause metadata <br> - Migrate vector store from local SQLite to RDS pgvector; rerun build index | 22/07/2026 | 22/07/2026 | Source code `vector_store/`, [Amazon RDS](https://000005.awsstudygroup.com/vi/) |
| 5 | - Create `legal_chunks` table with embedding vector(1536) column <br> - Optimize cosine search (IVFFlat index); configure connection pool and timeout | 23/07/2026 | 23/07/2026 | pgvector docs, `retriever.py` |
| 6 | - Complete Week 1 Worklog and Event 1 per FCAJ template <br> - Review report content; fix typos and Hugo formatting | 24/07/2026 | 24/07/2026 | [FCAJ Kick off](https://www.youtube.com/watch?v=AQlsd0nWdZk&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=1) |

### Week 5 Achievements

* Retrieval runs stably on RDS PostgreSQL pgvector.
* Understood CloudFormation, IAM Role, and full RDS lab stack.
* Submitted Week 1 Worklog and Event 1 on time.
