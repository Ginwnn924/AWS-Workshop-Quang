---
title: "API Deployment with Docker"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---
## Objective

Build Docker images, configure Docker Compose, and deploy on EC2.

## Steps

1. Build Docker image using Python 3.11-slim with CPU-only torch
2. Configure Docker Compose with `api` (port 8000) and `streamlit` (port 8501) services
3. Provision EC2 instance (t3.medium, Amazon Linux 2023)
4. Install Docker and Docker Compose on EC2
5. Clone repository, configure `.env`, build and launch
6. Initialize application database tables
7. Verify health check, API endpoint, and Streamlit UI
