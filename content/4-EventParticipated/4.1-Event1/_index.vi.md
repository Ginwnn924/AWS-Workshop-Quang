---
title: "Event 1"
date: 2026-08-08
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch sự kiện "Agent Forge - Deepdive Day 2"

**Thời gian:** 9:00–12:00, Thứ 7, 08/08/2026

**Địa điểm:** Tầng 26, Bitexco Financial Tower, 2 Đ. Hải Triều, Sài Gòn, Hồ Chí Minh 700000, Việt Nam

**Vai trò:** Người tham dự

**Các diễn giả:**

- Nghia Tran — Agentic SA
- Anh Pham — Cloud Consultant G-AsiaPacific Vietnam

## Nội dung chính

Phần lý thuyết bao gồm các chủ đề sau:

### Memory

- Memory giúp Agent lưu giữ thông tin, vượt qua giới hạn context window và cá nhân hóa trải nghiệm.
- **Short-term Memory:** lưu dữ liệu thô từ hội thoại, đồng bộ để truy xuất nhanh thông tin gần nhất.
- **Long-term Memory:** trích xuất insight và tri thức từ hội thoại, chuyển thành vector để lưu trữ lâu dài.
- **Memory Strategies:** gồm Summary, User Preference, Semantic và Episodic.
- **Namespace:** tổ chức dữ liệu theo cấu trúc phân cấp như `/Strategy/Actor/Session`, giúp thu hẹp phạm vi tìm kiếm, giảm token và tăng tốc truy xuất.

### Evaluations

- Evaluations đảm bảo Agent hoạt động chính xác, hữu ích và an toàn; phát hiện hallucination, lỗi reasoning và lựa chọn tool không phù hợp.
- Hai chế độ:
  - **On-demand Evaluation:** đánh giá chủ động trong quá trình development.
  - **Online Evaluation:** giám sát liên tục trong production thông qua telemetry và metrics.
- Đánh giá ở ba cấp:
  - **Session level** — toàn bộ phiên.
  - **Trace level** — từng response.
  - **Span level** — việc sử dụng tool và parameters.
- Hệ thống dùng **Judge** phân tích hoạt động Agent, đưa kết quả vào Observability để SME theo dõi và can thiệp.

### Observability

- Observability giúp developer hiểu, debug và tối ưu hoạt động bên trong Agent.
- Ba thành phần chính:
  - **Logs** — điều gì đã xảy ra.
  - **Traces** — quá trình xảy ra như thế nào.
  - **Metrics** — latency, token cost, error rate.
- Ngoài ra: OpenTelemetry, monitoring thời gian thực, alert và phân cấp dữ liệu Session → Trace → Span/Sub-span.

### AgentCore Components

- **Registry:** trung tâm quản lý và tái sử dụng Agent skills, tools và APIs; hỗ trợ Admin, Publisher và Consumer.
- **Harness:** framework tối giản khởi tạo Agent từ Model + System Prompt + Tool, hỗ trợ mở rộng.
- **Tools:** Agent tương tác hệ thống bên ngoài, thực hiện actions và truy cập dữ liệu/API thời gian thực.
- **Payments:** Agent thực hiện thanh toán; hiện hỗ trợ Stripe và Coinbase.
- **Optimization:** dùng dữ liệu Evaluation và Observability để cải thiện; hỗ trợ A/B testing, Red Teaming và self-optimizing loop.
- **Policy:** kiểm soát hành vi, bảo mật và compliance; Human-in-the-loop, Cedar, Strict/Permissive mode và Least Privilege.

### Phần thực hành

Hướng dẫn triển khai với **Agent SDK**, thiết lập **AWS Bedrock**, và dùng **CLI** để tạo project, deploy và test Agent trên AWS.

## Bài học rút ra

Qua sự kiện Agent Forge - Deepdive Day 2, em hiểu rõ hơn các thành phần cần thiết để xây dựng và vận hành AI Agent trong môi trường production — đặc biệt vai trò của Memory, Evaluations và Observability trong việc duy trì ngữ cảnh, đánh giá chất lượng và giám sát hoạt động.

Em cũng nắm cách các thành phần AgentCore (Registry, Harness, Tools, Policy, Optimization) phối hợp để quản lý, mở rộng, bảo mật và liên tục cải thiện Agent. Em nhận thức tầm quan trọng của Least Privilege và Human-in-the-loop khi kiểm soát hành động Agent.

Phần thực hành giúp em làm quen Agent SDK, AWS Bedrock và AWS CLI — từ khởi tạo project, triển khai đến kiểm thử Agent trên AWS.

## Một số hình ảnh khi tham gia sự kiện

![Tham gia Agent Forge Day 2](/images/4-Events/ev2-1.jpg)

![Tham gia Agent Forge Day 2](/images/4-Events/ev2-2.jpg)
