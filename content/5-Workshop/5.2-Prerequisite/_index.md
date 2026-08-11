---
title: "Prerequisites"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Prerequisites

Before setting up AWS services in the following sections, prepare the environment and minimum access as below.

## 1. Account and region

- Active AWS account
- Recommended region: **ap-southeast-1**
- Enable MFA for IAM user / root if not already enabled
- Track spend with AWS Budgets during labs

## 2. EC2 host

- Demo instance type: **t3a.small**
- OS: Amazon Linux or Ubuntu
- Enough disk for Docker images
- Subnet that can reach RDS and Bedrock, or has Internet egress when needed

## 3. Tools on EC2

Install and verify:

sudo yum install -y docker git
# or apt equivalent on Ubuntu
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
# re-login after adding the docker group
docker --version
git --version

## 4. Clone the codebase

git clone <your-repo-url> vietnamese-legal-llmops
cd vietnamese-legal-llmops
cp .env.sample .env

{{% notice warning %}}
Do not commit a .env file with RDS passwords, API keys, or Access Keys to Git.
{{% /notice %}}

## 5. Security groups

| Direction | Port | Notes |
| --- | --- | --- |
| Inbound EC2 | **8501** | Streamlit UI — open only from your IP or ALB |
| Inbound EC2 | **8000** | FastAPI — optional; usually Docker-internal only |
| Inbound RDS | **5432** | Allow only from the EC2 security group |

## 6. IAM and LLM

- Prefer an **IAM Instance Role** on EC2 for Bedrock / S3
- Avoid long-lived Access Keys on the instance when a role is possible
- Pick one LLM path:
  - **Bedrock** — enable model access in the region
  - **Gemini** — set GEMINI_API_KEY in .env for dev

## 7. Checklist before section 5.3

- [ ] Can sign in to AWS Console / CLI
- [ ] EC2 is ready; SSH or Session Manager works
- [ ] Docker and Git work on EC2
- [ ] vietnamese-legal-llmops is cloned with a .env file
- [ ] Security groups and IAM/role are minimally configured
- [ ] Bedrock or Gemini is chosen

Next: create an **S3 bucket** and upload data.
