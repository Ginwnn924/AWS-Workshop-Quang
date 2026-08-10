---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Research Notes on Serverless Real-time Voice AI on AWS

During my internship I mainly built **text** chat for the Law-Chatbot project. While reading AWS materials, I explored **voice AI** — letting users speak and hear responses in real time. This write-up captures what I learned from a serverless voice coaching pattern for field sales teams.

## Context and problem

From my reading, field sales organizations need **coaching** on products, cross-sell strategies, and handling customer objections. Managers cannot join every visit. A voice-based AI coach lets reps practice anytime through a web browser.

On the technical side, the challenge is **bidirectional audio** with low latency and many concurrent sessions, without paying for infrastructure that runs 24/7 when usage is sporadic.

## Architecture as I understand it

The data flow looks like this:

```
Browser (mic / speaker)
    ↔ AWS AppSync Events (2 pub/sub channels)
Bedrock AgentCore container
    ↔ Amazon Nova Sonic (voice model on Bedrock)
```

**AWS AppSync Events** carries real-time traffic. The client publishes audio to an upstream channel via HTTP POST; the container subscribes over WebSocket to receive it. AI responses go to a downstream channel; the client subscribes and plays audio.

What stood out to me is **separating transport from agent logic**: AWS handles WebSocket, routing, and scaling; the team focuses on the AgentCore container and Nova Sonic.

## Cost and operations model

From the article I compared pricing models: AppSync Events charges per event operation with **zero idle cost**. A ~5-minute coaching session costs roughly $0.007. A Fargate + ALB setup incurs baseline cost even when nobody is using it.

For intermittent sales usage, pay-per-use fits better than always-on infrastructure.

## Production lessons

The section I learned most from is a production debugging story: voice sessions stopped after ~112 seconds because **two independent bugs existed at once**:

1. The container lacked a `GET /ping` endpoint — AgentCore sends SIGKILL after ~120 seconds if health checks fail.
2. WebSocket subscribe used the wrong AppSync DNS — you must use the API-specific endpoint, not the generic regional URL.

Fixing only one bug still failed. CloudWatch logs showed 0 audio chunks while the client kept publishing — that clue led to the second issue.

My takeaway checklist for AgentCore voice deploys: always implement `/ping`, use correct WebSocket DNS, pass model IDs via environment variables, disable HTTP/2 timeouts for long streams, and log audio chunk counts per session.

## Connection to my internship project

Law-Chatbot uses text RAG on Bedrock. After studying this pattern, I see a path to add voice via Nova Sonic and AppSync Events while staying serverless — useful if we later support users who prefer speaking over typing for legal lookup.

## Conclusion

Production voice AI is not just calling speech-to-text and text-to-speech APIs. It requires real-time transport design, container health checks, and idle cost management. This pattern applies to customer service, training, and field assistants — not only sales coaching.

### Reference

- [Serverless Real-time Voice AI on AWS: A Pattern for Enterprise Sales Coaching](https://aws.amazon.com/blogs/industries/serverless-real-time-voice-ai-on-aws-a-pattern-for-enterprise-sales-coaching/)
- [Facebook post (AWS Study Group)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2240599400038353&hoisted_section_header_type=recently_seen)
