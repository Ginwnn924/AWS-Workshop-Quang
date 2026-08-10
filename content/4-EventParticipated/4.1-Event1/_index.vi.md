---
title: "Event 1"
date: 2026-08-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch sự kiện "Agent Forge - Deepdive Day 1"

### 1. Tổng quan sự kiện

- **Thời gian:** 9:00–12:00, Thứ 7, 01/08/2026
- **Địa điểm:** Tầng 26, Bitexco Financial Tower, 2 Đ. Hải Triều, Sài Gòn, Hồ Chí Minh 700000, Việt Nam
- **Vai trò:** Người tham dự

### 2. Danh sách diễn giả

- **Nghia Tran** — Agentic SA
- **Anh Pham** — Cloud Consultant G-AsiaPacific Vietnam

### 3. Nội dung chính

Workshop nâng cao (L300) về **Amazon Bedrock AgentCore**, hướng tới doanh nghiệp xây dựng hệ thống AI tự chủ (Agentic AI) ở quy mô production.

#### Phần lý thuyết

- **Giới thiệu Agentic AI:** khái niệm AI tự chủ, khả năng lập kế hoạch và thực thi từng bước; các mức độ autonomy từ *deterministic workflow* đến *fully autonomous*.
- **Amazon Bedrock AgentCore:** dịch vụ quản lý, triển khai và vận hành AI agent; tuân thủ tiêu chuẩn về hiệu năng, khả năng mở rộng và bảo mật.
- **Thành phần cốt lõi AgentCore:**
  - **Runtime Environment:** môi trường serverless chạy agent, dùng Firecracker MicroVM để cô lập tài nguyên.
  - **Identity:** lớp xác thực và phân quyền với JWT và Workload Access Token.
  - **Gateway:** middleware kết nối agent với tools qua Model Context Protocol (MCP); hỗ trợ kiểm soát tập trung và human-in-the-loop.
- **Bảo mật & kết nối:** kết nối agent vào VPC qua AWS PrivateLink — dữ liệu nội bộ không lộ ra internet công cộng.

#### Phần thực hành

Hướng dẫn thực hành **Runtime**, **Gateway** và **Identity** đã giới thiệu ở lý thuyết; thiết lập môi trường cần thiết cho các bài lab.

### 4. Bài học rút ra

Qua sự kiện **Agent Forge - Deepdive Day 1**, em hiểu rõ hơn khái niệm Agentic AI và cách xây AI Agent có khả năng tự lập kế hoạch, thực thi task và tương tác dịch vụ bên ngoài.

Em nắm kiến trúc Amazon Bedrock AgentCore — Runtime, Identity, Gateway — và vai trò từng thành phần khi deploy và vận hành agent. Em nhận thức tầm quan trọng bảo mật khi triển khai AI: kết hợp Amazon VPC và AWS PrivateLink.

Phần thực hành giúp em làm quen quy trình cấu hình môi trường và triển khai Bedrock AgentCore — nối lý thuyết với ứng dụng thực tế.

### Một số hình ảnh khi tham gia sự kiện

![Tham gia Agent Forge Day 1](/images/4-Events/ev1-1.jpg)
