---
title: "Clean Up Resources"
date: 2026-08-11
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---
## Clean Up Resources

To avoid ongoing charges, delete resources in the correct dependency order:

1. Docker Compose down on EC2
2. Terminate EC2 instance
3. Delete RDS PostgreSQL (skip final snapshot for dev)
4. Delete DB Subnet Group
5. Empty S3 bucket, then delete CloudFormation stack (removes Cognito, DynamoDB, SQS)
6. Delete Lambda function
7. Delete VPC resources (security groups, subnets, IGW, VPC)
8. Delete CloudWatch log groups

## Verification

Check that no running EC2 instances, RDS databases, or active CloudFormation stacks remain in Region `ap-southeast-1`.
