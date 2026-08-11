---
title: "Application UI & Features Overview"
date: 2026-08-11
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---
## User Interfaces

- **Streamlit**: Full-featured app with login, register, chatbot, and admin pages
- **Chainlit**: Lightweight chat interface for quick demos

## Key Workflows

1. **Legal Q&A**: Login → Ask question → View answer with citations → Rate feedback
2. **Document Upload**: Admin uploads PDF → S3 → SQS → Lambda → Available in search
3. **User Management**: Admin manages users via Cognito (enable/disable/groups)

## API Endpoints

POST `/ask`, POST `/api/chat`, GET `/api/conversations`, GET `/api/admin/users`, POST `/api/admin/documents`, DELETE `/api/admin/documents/{id}`, GET `/health`
