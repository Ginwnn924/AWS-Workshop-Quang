---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Research Notes on Priority-Based Message Processing with Amazon MQ and AWS App Runner

## Context and problem

From my reading, many enterprise systems cannot treat every message as plain FIFO. Rush orders from key customers, production-critical alerts, or time-sensitive workflow steps need **immediate attention**, while routine tasks (reports, data sync) can wait.

The technical problem breaks down into three parts:

- **Conditional delay:** normal messages may wait seconds or minutes before entering the queue; urgent messages must **bypass delay** and queue immediately.
- **Priority routing:** the broker must ensure consumers process high-priority messages before low-priority ones.
- **Real-time feedback:** users need visible processing status (waiting, in progress, done) — not just fire-and-forget with polling.

The solution in the article uses AWS managed services to reduce infrastructure operations, with Infrastructure as Code (AWS CDK) for deployments from dev through production.

## Architecture as I understand it

The demo combines **Amazon MQ** (ActiveMQ message broker), **Amazon DynamoDB** (status persistence), **AWS App Runner** (containerized Spring Boot app), and **WebSocket** (real-time updates via DynamoDB Streams + API Gateway).

![Priority-based message processing architecture with Amazon MQ, DynamoDB, and App Runner](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2025/11/13/image-1-2.jpeg)

*Figure 1 — Solution architecture overview*

Three priority flows (JMS priority levels):

| Level | JMS Priority | Behavior |
|-------|--------------|----------|
| High | 9 | Bypass delay, queue immediately — express lane |
| Standard | 4 | Wait for configured delay, then queue |
| Low | 0 | Process after all higher-priority messages |

The overall flow I noted:

1. Client sends a request via REST API (Spring Boot on App Runner).
2. App classifies priority → high goes straight to queue; standard/low may delay first (demo uses CompletableFuture at app layer).
3. Message publishes to **Amazon MQ** (ActiveMQ) with setJMSPriority().
4. Consumer processes in priority order; updates status in **DynamoDB**.
5. **DynamoDB Streams** triggers Lambda → pushes updates through **API Gateway WebSocket** → React frontend shows live status.

Amazon MQ provides persistence, failover, and DLQ for failed messages. App Runner auto-scales containers by traffic — no manual EC2/ECS management.

## Priority paths in detail

**High-priority path:** critical messages skip delay and publish to the queue immediately with priority 9. Suitable for alerts, rush orders, events that cannot wait.

![High-priority flow — bypass delay](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2025/11/13/image-3-4.jpeg)

*Figure 2 — High-priority message flow*

**Standard-priority path:** messages wait for a configured delay period, then enter the queue with priority 4. Uses JMS async processing — does not block the main thread.

![Standard-priority flow — delay before queue](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2025/11/13/image-2-6.png)

*Figure 3 — Standard-priority message flow*

**Low-priority path:** priority 0 — consumers process after higher-priority messages are cleared.

A note from the article: the demo uses **application-level delay** (CompletableFuture) to illustrate bypass behavior. Production should use native Amazon MQ/ActiveMQ delay features — AMQ_SCHEDULED_DELAY, delay queues, TTL — for better scalability.

## Real-time UI and reliability

What stood out is combining **message queues** with **real-time feedback**:

- Bidirectional WebSocket between React frontend and API Gateway.
- DynamoDB Streams (CDC) captures every status change.
- Lambda forwards events to connected clients.

Users see processing timelines, queue statistics by priority, and proof that high-priority messages "jump ahead" of standard ones even when sent later.

![Demo web UI — real-time updates via WebSocket](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2025/11/13/image-4-3.jpeg)

*Figure 4 — UI validation with live status updates*

Retry: the article mentions **dual-layer retry** — app layer plus MQ DLQ — so messages are not lost when consumers fail temporarily.

## Deployment and operations notes

Demo stack: Java 17, Spring Boot 3.2, React frontend, Docker → ECR → App Runner. Infrastructure via **AWS CDK** (Python): DynamoDB (on-demand, KMS encryption, status GSI, Streams enabled), Amazon MQ broker (ActiveMQ 5.18, private subnet), App Runner service with env vars pointing to DynamoDB and MQ.

Security from the article:
- IAM least privilege for App Runner role.
- MQ in private VPC, not publicly accessible.
- KMS encryption at rest (DynamoDB + MQ); TLS for ActiveMQ connections.
- Security groups open only required ports (e.g. 61617 SSL).

Estimated cost (US East, from article): ~$53–95/month for 1,000 msg/day; scaling to 100,000 msg/day around $850–1,570 — mainly from MQ broker size and App Runner.

Monitoring: CloudWatch dashboards by priority, queue depth, DynamoDB throttling, custom success rate/retry metrics.

## Connection to my internship project

Law-Chatbot currently uses ingestion Lambda for legal documents in batch — we do not have explicit priority queues yet. This article suggests expansion paths:

- **High priority:** newly published laws — re-index immediately for timely lookup.
- **Standard:** periodic re-index or low-traffic documents.
- **Low:** cleanup, statistics, backup jobs.

Combining Amazon MQ + DynamoDB status + WebSocket could give admins a real-time ingestion pipeline dashboard — instead of only CloudWatch logs.

I also connect this to **async processing** patterns: separate heavy work from synchronous requests; the difference here is **priority routing** at the message broker layer, not just SNS fan-out.

## Conclusion

Priority-based message processing is not just "add a priority field to JSON." It requires a broker with JMS priority support, delay/bypass strategy, persistence + DLQ, and (for good UX) real-time status via Streams + WebSocket.

The pattern fits e-commerce, alert systems, and workflow engines — any domain with urgent and routine tasks sharing one pipeline. For small teams, Amazon MQ + App Runner + CDK enables a working prototype without self-managing a message broker on EC2.

Sample repo and IaC are on GitHub (per the original post) — useful for lab work after the internship.

### Reference

- [Build priority-based message processing with Amazon MQ and AWS App Runner](https://aws.amazon.com/blogs/architecture/build-priority-based-message-processing-with-amazon-mq-and-aws-app-runner/)
- [Facebook post (AWS Study Group)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2240621456702814&hoisted_section_header_type=recently_seen)
