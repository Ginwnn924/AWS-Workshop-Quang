---
title: "Foundation Infrastructure (CloudFormation)"
date: 2026-08-11
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---
## Objective

Provision all foundation infrastructure using a single CloudFormation template (`infra/foundation.yaml`): Cognito User Pool, DynamoDB Table, S3 Bucket, and SQS Queue with Dead Letter Queue.

## Steps

1. Navigate to CloudFormation → Create stack → Upload `infra/foundation.yaml`
2. Configure parameters: ProjectName, Environment, AdminEmail
3. Acknowledge IAM resource creation and create stack
4. Record Outputs for `.env` configuration
5. Verify resources using AWS CLI
