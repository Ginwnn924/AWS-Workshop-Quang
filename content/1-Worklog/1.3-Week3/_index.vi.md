---
title: "Worklog Tuần 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3

* Chốt đề tài và hoàn thành Proposal cho dự án Law-Chatbot.
* Hoàn thiện luồng RAG và giao diện Chainlit.
* Kiểm thử hỏi đáp và tinh chỉnh prompt trích dẫn nguồn luật.

### Các công việc triển khai trong tuần

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Họp nhóm chốt đề tài Vietnamese Legal Chatbot <br> - Viết Proposal: mục tiêu, phạm vi, use case, lộ trình và dịch vụ AWS dự kiến | 06/07/2026 | 06/07/2026 | [Amazon Bedrock Baseline Architecture](https://aws.amazon.com/vi/blogs/architecture/amazon-bedrock-baseline-architecture-in-an-aws-landing-zone/) |
| Thứ 3 | - Vẽ sơ đồ kiến trúc RAG Serverless <br> - Cập nhật tài liệu `AWS_chatbot.txt` mô tả 3 luồng chính | 07/07/2026 | 07/07/2026 | [Draw.io Guide](https://www.youtube.com/watch?v=l8isyDe-GwY&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=2) |
| Thứ 4 | - Hoàn thiện `retriever`, `prompt`, `generator`, `qa_service` <br> - Cấu hình LLM provider: Gemini / Bedrock | 08/07/2026 | 08/07/2026 | Source code `rag_core/` |
| Thứ 5 | - Tích hợp Chainlit làm giao diện chat; stream câu trả lời realtime <br> - Thiết kế prompt yêu cầu trích dẫn điều/khoản nguồn luật | 09/07/2026 | 09/07/2026 | Chainlit docs, `prompt.py` |
| Thứ 6 | - Xử lý Git submodule, IAM, GitHub cho repo nhóm <br> - Kiểm thử hỏi đáp; chỉnh prompt và top_k theo kết quả retrieval | 10/07/2026 | 10/07/2026 | GitHub repo dự án <br> [AWS IAM](https://000002.awsstudygroup.com/vi/) |

### Kết quả đạt được tuần 3

* Hoàn thành Proposal và sơ đồ kiến trúc RAG Serverless.
* Luồng RAG end-to-end chạy với Chainlit; câu trả lời kèm chunk nguồn.
* Repo GitHub ổn định; team thống nhất kiến trúc cloud cho Law-Chatbot.
