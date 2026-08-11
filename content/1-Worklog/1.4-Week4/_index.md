---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives

* Practice AWS security labs and deploy applications on EC2.
* Dockerize FastAPI/Chainlit for deployment environments.
* Design VPC architecture and VPC Endpoints for the project.

### Tasks carried out this week

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Monday | - Practice IAM lab: users, groups, policies; enable MFA and least privilege <br> - Cognito User Pool lab; basic Security Hub scan <br> - Launch EC2 Linux, attach EBS Snapshot, create Custom AMI | 13/07/2026 | 13/07/2026 | [AWS IAM](https://000002.awsstudygroup.com/vi/) <br> [AWS Cognito](https://000081.awsstudygroup.com/vi/) |
| Tuesday | - Deploy Node.js app on EC2 per FCAJ lab <br> - Configure IAM Instance Profile for EC2 AWS service access | 14/07/2026 | 14/07/2026 | [Cloud Journey](https://cloudjourney.awsstudygroup.com/) |
| Wednesday | - Build FastAPI REST API: **/chat**, **/health** endpoints, CORS <br> - Deploy FastAPI on EC2; verify RDS pgvector connection from private subnet | 15/07/2026 | 15/07/2026 | FastAPI docs, source code api/ |
| Thursday | - Write multi-stage Dockerfile for FastAPI + Chainlit UI <br> - Build/push image; run container on EC2 with .env variables | 16/07/2026 | 16/07/2026 | deploy/Dockerfile, Docker docs |
| Friday | - Design 3-tier VPC: public, private, isolated <br> - Configure Gateway Endpoint and Interface Endpoint | 17/07/2026 | 17/07/2026 | [Amazon VPC](https://000003.awsstudygroup.com/vi/) <br> [Workshop Overview](/5-workshop/5.1-workshop-overview/) |

### Week 4 Achievements

* Completed IAM/Cognito security labs and application deployment on EC2 Linux.
* FastAPI and Docker image ready for cloud deployment.
* 3-tier VPC architecture and VPC Endpoints aligned within the team.
