---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Nghiên cứu xử lý message theo priority với Amazon MQ và AWS App Runner

## Bối cảnh và bài toán

Theo em hiểu, nhiều hệ thống doanh nghiệp không thể xử lý mọi message theo thứ tự FIFO thuần. Ví dụ: đơn rush từ khách VIP, alert production critical, hay bước trong quy trình nghiệp vụ time-sensitive — cần **ưu tiên ngay**, trong khi task routine (báo cáo, đồng bộ dữ liệu) có thể chờ.

Bài toán kỹ thuật em rút ra gồm ba phần:

- **Delay có điều kiện:** message thường có thể chờ vài giây/phút trước khi vào queue; message khẩn phải **bỏ qua delay** và vào queue ngay.
- **Routing theo priority:** broker phải đảm bảo consumer xử lý message high trước low.
- **Phản hồi real-time:** user cần thấy trạng thái xử lý (đang chờ, đang xử lý, hoàn thành) — không chỉ gửi request rồi chờ polling.

Giải pháp trong bài dùng các dịch vụ AWS managed để giảm vận hành hạ tầng, kèm IaC (AWS CDK) deploy từ dev đến production.

## Kiến trúc em tóm lược được

Hệ thống demo kết hợp **Amazon MQ** (message broker ActiveMQ), **Amazon DynamoDB** (lưu trạng thái), **AWS App Runner** (chạy app Spring Boot containerized), và **WebSocket** (cập nhật real-time qua DynamoDB Streams + API Gateway).

![Kiến trúc priority-based message processing với Amazon MQ, DynamoDB và App Runner](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2025/11/13/image-1-2.jpeg)

*Hình 1 — Tổng quan kiến trúc giải pháp*

Ba luồng priority (JMS priority levels):

| Mức | JMS Priority | Hành vi |
|-----|--------------|---------|
| High | 9 | Bỏ qua delay, vào queue ngay — "express lane" |
| Standard | 4 | Chờ delay cấu hình rồi mới vào queue |
| Low | 0 | Xử lý sau khi hết message priority cao hơn |

Luồng tổng quát em ghi chú:

1. Client gửi request qua REST API (Spring Boot trên App Runner).
2. App phân loại priority → high đi thẳng queue; standard/low có thể delay trước (demo dùng CompletableFuture ở app layer).
3. Message publish lên **Amazon MQ** (ActiveMQ) với setJMSPriority().
4. Consumer xử lý theo thứ tự priority; cập nhật trạng thái vào **DynamoDB**.
5. **DynamoDB Streams** trigger Lambda → đẩy update qua **API Gateway WebSocket** → React frontend hiển thị live.

Amazon MQ cung cấp persistence, failover, DLQ cho message fail. App Runner auto-scale container theo traffic — không cần quản lý EC2/ECS thủ công.

## Ba luồng priority chi tiết

**High-priority path:** message critical bỏ qua cơ chế delay, publish queue ngay với priority 9. Phù hợp alert, đơn gấp, sự kiện không thể trễ.

![Luồng high-priority — bypass delay](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2025/11/13/image-3-4.jpeg)

*Hình 2 — Luồng message ưu tiên cao*

**Standard-priority path:** message chờ delay period (cấu hình per message type) rồi mới vào queue với priority 4. Dùng JMS async processing — không block thread chính.

![Luồng standard-priority — có delay trước khi queue](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2025/11/13/image-2-6.png)

*Hình 3 — Luồng message priority chuẩn*

**Low-priority path:** priority 0 — consumer xử lý sau khi queue đã hết message cao hơn.

Em chú ý lưu ý trong bài: demo dùng **application-level delay** (CompletableFuture) để minh họa bypass behavior. Production nên dùng tính năng delay native của Amazon MQ/ActiveMQ — AMQ_SCHEDULED_DELAY, delay queues, TTL — ổn định hơn khi scale.

## Real-time UI và reliability

Phần em thấy hay là kết hợp **message queue** với **real-time feedback**:

- WebSocket hai chiều giữa React frontend và API Gateway.
- DynamoDB Streams (CDC) bắt mọi thay đổi trạng thái item.
- Lambda forward event tới client đang kết nối.

User thấy timeline xử lý, thống kê queue theo priority, và chứng minh high-priority message "nhảy" trước standard dù gửi sau.

![Giao diện web demo — cập nhật real-time qua WebSocket](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2025/11/13/image-4-3.jpeg)

*Hình 4 — UI validation với live status updates*

Retry: bài đề cập **dual-layer retry** — app layer + DLQ trên MQ — để message không mất khi consumer fail tạm thời.

## Triển khai và vận hành em ghi chú

Stack demo: Java 17, Spring Boot 3.2, React frontend, Docker → ECR → App Runner. Infrastructure bằng **AWS CDK** (Python): DynamoDB (on-demand, KMS encryption, GSI theo status, Streams enabled), Amazon MQ broker (ActiveMQ 5.18, private subnet), App Runner service với env vars trỏ DynamoDB và MQ endpoint.

Bảo mật theo bài:
- IAM least privilege cho App Runner role.
- MQ trong VPC private, không public.
- KMS encryption at rest (DynamoDB + MQ); TLS cho kết nối ActiveMQ.
- Security group chỉ mở port cần thiết (ví dụ 61617 SSL).

Chi phí ước tính (US East, theo bài): ~$53–95/tháng cho 1.000 msg/ngày; scale lên 100.000 msg/ngày khoảng $850–1.570 — chủ yếu từ MQ broker size và App Runner.

Monitoring: CloudWatch dashboard theo priority, queue depth, DynamoDB throttling, custom metric success rate/retry count.

## Liên hệ với dự án thực tập

Law-Chatbot hiện ingestion Lambda xử lý tài liệu pháp luật theo batch — em chưa có priority queue rõ ràng. Bài này gợi ý hướng mở rộng:

- **High priority:** cập nhật văn bản luật mới ban hành — re-index ngay để user tra cứu kịp.
- **Standard:** re-index định kỳ hoặc tài liệu ít truy cập.
- **Low:** job dọn dẹp, thống kê, backup.

Kết hợp Amazon MQ + DynamoDB status + WebSocket có thể cho admin dashboard theo dõi pipeline ingestion real-time — thay vì chỉ xem CloudWatch log.

Em cũng liên hệ với pattern **async processing** đã học ở bài Pelago (Blog khác): tách xử lý nặng khỏi request đồng bộ; khác biệt là bài này thêm **priority routing** ở tầng message broker thay vì chỉ SNS fan-out.

## Kết luận

Priority-based message processing không chỉ là "thêm field priority vào JSON". Cần broker hỗ trợ JMS priority, chiến lược delay/bypass, persistence + DLQ, và (nếu cần UX tốt) real-time status qua Streams + WebSocket.

Pattern phù hợp e-commerce, alert system, workflow engine — bất kỳ domain nào có task khẩn và task thường chung một pipeline. Với team nhỏ, Amazon MQ + App Runner + CDK giúp có prototype chạy được mà không tự vận hành message broker trên EC2.

Repo mẫu và IaC có trên GitHub (theo bài gốc) — hữu ích nếu em muốn lab thêm sau kỳ thực tập.

### Reference

- [Build priority-based message processing with Amazon MQ and AWS App Runner](https://aws.amazon.com/blogs/architecture/build-priority-based-message-processing-with-amazon-mq-and-aws-app-runner/)
- [Bài chia sẻ trên Facebook (AWS Study Group)](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2240621456702814&hoisted_section_header_type=recently_seen)
