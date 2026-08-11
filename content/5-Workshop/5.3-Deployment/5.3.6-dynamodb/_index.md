---
title: "DynamoDB — Chat history"
date: 2026-08-11
weight: 6
chapter: false
pre: " <b> 5.3.6. </b> "
---

# DynamoDB — Chat history

DynamoDB stores conversation metadata and messages for the authenticated API (`/api/chat`, `/api/conversations`).

{{< mermaid >}}
graph TB
    U["User"] --> C["Conversation"]
    C --> M1["Message user"]
    C --> M2["Message assistant"]
    C --> GSI1["GSI by user"]
    C --> GSI2["GSI by date (admin)"]
    C --> TTL["TTL expiry"]
{{< /mermaid >}}

| Item type | Key pattern | Content |
| --- | --- | --- |
| Conversation | `PK=USER#<id>`, `SK=CONV#<conv_id>` | Title, timestamp |
| Message | `SK=MSG#<timestamp>` | Role, content, metadata |

## Steps

1. Create DynamoDB table (via CloudFormation or manually)
2. Configure `.env` with table name
3. Test: POST /api/chat creates conversation + messages
4. Verify TTL auto-deletion

Refer to the Vietnamese version for full details.
