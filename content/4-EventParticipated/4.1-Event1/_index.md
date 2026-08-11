---
title: "Event 1"
date: 2026-08-08
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Event Summary: "Agent Forge - Deepdive Day 2"

**Time:** 9:00 AM – 12:00 PM, Saturday, August 8, 2026

**Location:** 26th Floor, Bitexco Financial Tower, 2 Hai Trieu Street, Saigon, Ho Chi Minh City 700000, Vietnam

**Role:** Attendee

**Speakers:**

- Nghia Tran — Agentic SA
- Anh Pham — Cloud Consultant G-AsiaPacific Vietnam

## Main content

The theory session covered the following topics:

### Memory

- Memory helps agents retain information, go beyond context window limits, and personalize experiences.
- **Short-term Memory:** stores raw conversation data for fast retrieval of recent context.
- **Long-term Memory:** extracts insights and knowledge, stored as vectors for long-term use.
- **Memory Strategies:** Summary, User Preference, Semantic, and Episodic.
- **Namespace:** hierarchical organization (e.g. **/Strategy/Actor/Session**) to narrow search scope, reduce tokens, and speed up retrieval.

### Evaluations

- Evaluations ensure agents are accurate, useful, and safe; they detect hallucination, reasoning errors, and inappropriate tool use.
- Two modes:
  - **On-demand Evaluation:** proactive assessment during development.
  - **Online Evaluation:** continuous production monitoring via telemetry and metrics.
- Three evaluation levels:
  - **Session level** — entire session.
  - **Trace level** — each response.
  - **Span level** — tool usage and parameters.
- A **Judge** analyzes agent activity; results feed into Observability for SME review and intervention.

### Observability

- Observability helps developers understand, debug, and optimize agent internals.
- Three core components:
  - **Logs** — what happened.
  - **Traces** — how it happened.
  - **Metrics** — latency, token cost, error rate.
- Also: OpenTelemetry, real-time monitoring, alerts, and data hierarchy Session → Trace → Span/Sub-span.

### AgentCore Components

- **Registry:** central management and reuse of agent skills, tools, and APIs (Admin, Publisher, Consumer).
- **Harness:** minimal framework to bootstrap agents from Model + System Prompt + Tool.
- **Tools:** external system interaction, actions, and real-time data/API access.
- **Payments:** agent payments; Stripe and Coinbase supported.
- **Optimization:** uses evaluation and observability data for A/B testing, red teaming, and self-optimizing loops.
- **Policy:** behavior, security, and compliance controls; human-in-the-loop, Cedar, strict/permissive modes, least privilege.

### Hands-on session

Deployment with **Agent SDK**, **AWS Bedrock** setup, and **CLI** workflows to create projects, deploy, and test agents on AWS.

## Lessons learned

Through Agent Forge - Deepdive Day 2, I gained a clearer picture of what it takes to build and operate AI agents in production — especially the roles of Memory, Evaluations, and Observability for context, quality assessment, and monitoring.

I also understand how AgentCore components (Registry, Harness, Tools, Policy, Optimization) work together to manage, scale, secure, and continuously improve agents. Least privilege and human-in-the-loop stood out as critical for controlling agent actions.

The hands-on portion introduced Agent SDK, AWS Bedrock, and AWS CLI — from project setup through deployment and testing on AWS.

## Event photos

![Agent Forge Day 2](images/4-Events/ev2-1.jpg)

![Agent Forge Day 2](images/4-Events/ev2-2.jpg)
