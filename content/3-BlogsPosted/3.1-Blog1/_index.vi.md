---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Nghiên cứu Voice AI real-time serverless trên AWS

## Bối cảnh và bài toán

Theo em hiểu, doanh nghiệp bán hàng hiện trường cần được **huấn luyện** về sản phẩm, chiến lược cross-sell và cách xử lý phản đối của khách. Không phải lúc nào manager cũng đi cùng rep. Một AI coach bằng giọng nói cho phép nhân viên luyện tập bất cứ lúc nào qua trình duyệt.

Phía kỹ thuật, thách thức là truyền **audio hai chiều** với độ trễ thấp, đồng thời scale được nhiều phiên làm việc mà không muốn trả chi phí hạ tầng chạy 24/7 khi ít người dùng.

## Kiến trúc em tóm lược được

Luồng dữ liệu có thể hình dung như sau:

Trình duyệt (mic / loa)
    ↔ AWS AppSync Events (2 kênh pub/sub)
Container trên Bedrock AgentCore
    ↔ Amazon Nova Sonic (model giọng nói trên Bedrock)

**AWS AppSync Events** đóng vai trò vận chuyển real-time. Client gửi audio lên kênh upstream qua HTTP POST; container subscribe WebSocket để nhận. Phản hồi AI được đẩy xuống kênh downstream; client subscribe và phát qua loa.

Điểm em thấy quan trọng là **tách lớp transport và logic agent**: AWS lo WebSocket, routing, scale; team dev tập trung vào container AgentCore và model Nova Sonic.

## Chi phí và mô hình vận hành

Em so sánh nhanh trong bài đọc: AppSync Events tính phí theo số lần thao tác sự kiện, không có chi phí idle. Một phiên coaching khoảng 5 phút ước tính ~$0.007. Trong khi đó, mô hình Fargate kèm ALB luôn phát sinh chi phí nền dù không có ai dùng.

Với use case sales — dùng gián đoạn, không liên tục — mô hình pay-per-use phù hợp hơn hạ tầng always-on.

## Bài học vận hành production

Phần em học nhiều nhất là câu chuyện debug production: phiên voice bị dừng sau khoảng 112 giây vì **hai lỗi độc lập cùng tồn tại**:

1. Container thiếu endpoint **GET /ping** — AgentCore gửi SIGKILL sau ~120 giây nếu health check không trả 200.
2. WebSocket subscribe sai DNS AppSync — phải dùng endpoint theo API ID, không dùng URL regional chung.

Sửa một lỗi vẫn fail. Log CloudWatch ghi nhận 0 audio chunk dù client vẫn gửi — đó là manh mối để lần ra lỗi thứ hai.

Từ đó em rút ra checklist khi deploy voice trên AgentCore: luôn có **/ping**, dùng đúng DNS WebSocket, cấu hình model ID qua biến môi trường, tắt timeout HTTP/2 cho stream dài, và log số chunk audio theo session.

## Liên hệ với dự án thực tập

Law-Chatbot của chúng em dùng RAG text trên Bedrock. Sau khi đọc pattern này, em thấy có thể mở rộng sang voice bằng Nova Sonic và AppSync Events mà vẫn giữ hướng serverless — hữu ích nếu sau này hỗ trợ người dùng không quen gõ phím khi tra cứu pháp luật.

## Kết luận

Voice AI production không chỉ là gọi API speech-to-text rồi text-to-speech. Cần thiết kế transport real-time, health check container, và quản lý chi phí idle. Pattern này có thể áp dụng cho customer service, đào tạo, hoặc trợ lý hiện trường — không giới hạn riêng sales coaching.

### Reference

- [Serverless Real-time Voice AI on AWS: A Pattern for Enterprise Sales Coaching](https://aws.amazon.com/blogs/industries/serverless-real-time-voice-ai-on-aws-a-pattern-for-enterprise-sales-coaching/)
- [Bài chia sẻ trên Facebook (AWS Study Group)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2240599400038353&hoisted_section_header_type=recently_seen)
