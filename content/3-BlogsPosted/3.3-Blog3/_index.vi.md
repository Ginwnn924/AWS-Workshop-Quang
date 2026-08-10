---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Nghiên cứu giảm gian lận SMS OTP với Vonage và Amazon Cognito

## Bối cảnh và bài toán

Xác thực người dùng là điểm bị tấn công nhiều nhất trong ứng dụng web/mobile. Theo số liệu trong bài, chi phí tội phạm mạng toàn cầu có thể đạt **23 nghìn tỷ USD vào 2027**; **20%** gian lận liên quan danh tính giả và khai thác authentication; account takeover (ATO) tăng **141%** kể từ 2021.

Nhưng vấn đề không chỉ là bảo mật. **SMS OTP** (mã xác thực một lần qua tin nhắn) chỉ đạt khoảng **80% conversion** — tức **1/5 user hợp lệ bỏ cuộc** ngay bước xác minh. Doanh nghiệp còn gánh hàng trăm nghìn ticket helpdesk mỗi năm vì user không nhận được hoặc nhập sai mã.

Em hiểu thông điệp chính của bài: lâu nay người ta nghĩ **bảo mật cao = trải nghiệm kém**. Vonage lập luận điều đó là giới hạn của công cụ cũ, không phải quy luật bất biến. Dữ liệu **real-time từ nhà mạng (MNO)** có thể vừa tăng độ tin cậy danh tính, vừa giảm ma sát cho user.

## Network-powered khác gì công cụ truyền thống?

Hầu hết tín hiệu chống gian lận hiện nay dựa trên dữ liệu **tổng hợp, cache hoặc hành vi** — tra cứu số điện thoại từ database cập nhật chậm vài ngày/tuần, device fingerprinting có thể giả mạo, behavioral biometrics cần lịch sử session.

Vonage **network-powered** truy vấn **trực tiếp nhà mạng di động** theo thời gian thực:

- Kiểm tra **SIM swap** → hỏi chính mạng đã thực hiện đổi SIM chưa.
- **Silent Authentication** → chứng minh sở hữu SIM qua **phiên cellular data**, không qua SMS.

Với ATO bằng SIM swap, “gần đây” có nghĩa là **vài phút đến vài giờ** — database tĩnh refresh hàng tuần chỉ ghi nhận sau sự kiện, không chặn kịp. Real-time operator query đóng cửa sổ đó.

## Ba trụ cột giải pháp Vonage

Vonage gom ba API thành stack bảo mật tích hợp với Cognito qua luồng **`CUSTOM_AUTH`**:

### 1. Identity Insights — kiểm tra trước khi gửi OTP

Chạy **trước** khi khởi tạo kênh xác minh. Một số tín hiệu em ghi chú:

| Tín hiệu | Ý nghĩa theo em hiểu |
|----------|----------------------|
| `format`, `network_type` | Lọc số không hợp lệ, VoIP, điện thoại bàn, số premium-rate — thường dùng tạo tài khoản giả |
| `sim_swap` | Phát hiện đổi SIM trong khoảng thời gian cấu hình — dấu hiệu ATO đang diễn ra |
| `subscriber_match` | So khớp tên/địa chỉ với hồ sơ KYC của nhà mạng |
| `device_swap` *(sắp có)* | Đổi thiết bị gắn với số — có thể kẻ xấu đã chiếm SIM |
| `recycled_number` *(sắp có)* | Số đã thu hồi và cấp lại — rủi ro onboarding nhầm danh tính |

Kết quả pre-check kích hoạt **chính sách rủi ro**: step-up challenge, chặn cứng, hoặc log im lặng. Gian lận bị chặn **trước khi gửi một OTP nào** — tiết kiệm chi phí SMS và xử lý fraud.

### 2. Verify + Silent Authentication — giảm “thuế ma sát”

Bài gọi tổn thất tích lũy từ mỗi bước xác thực thêm là **friction tax**. Với SMS OTP ~80% hoàn thành, ~20% user hợp lệ rời bỏ trước khi vào app.

**Silent Authentication:** khi user đăng nhập từ điện thoại, Vonage route HTTP request qua **kết nối 4G/5G** của user. Nhà mạng xác nhận SIM đăng ký với số điện thoại khớp session — diễn ra **nền, dưới 5 giây**, user **không gõ mã**.

Nếu Silent Auth không khả dụng, Verify **fallback** tự động sang SMS, RCS, Voice, WhatsApp hoặc email — user không cần biết chi tiết kỹ thuật.

Lợi ích em thấy rõ: giảm ba vector tấn công SMS OTP phổ biến — **SIM swap** (kẻ xấu nhận mã), **SS7 interception** (chặn tin nhắn trên đường truyền), **social engineering** (lừa user đọc mã).

### 3. Fraud Defender — bảo vệ kênh gửi OTP

Chống **AIT (Artificially Inflated Traffic)** và **SMS pumping** — bot gửi hàng loạt OTP tới số premium-rate do kẻ xấu kiểm soát, phát sinh chi phí lớn mà khó phát hiện.

Fraud Defender giám sát và chặn real-time tại điểm gửi. Vonage công bố khách hàng tiết kiệm **hơn 3 triệu USD** chi phí fraud SMS; với khách dùng Verify API, Fraud Defender **bật sẵn không phí thêm**.

## Kiến trúc tích hợp Amazon Cognito

![Sơ đồ kiến trúc Risk-Adaptive Sign-In: CloudFront/WAF → API Gateway → Cognito → Lambda → Vonage → MNO](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2026/06/09/ARCHBLOG-1533-1.png)

*Hình 1 — Kiến trúc xác thực thích ứng rủi ro với Cognito và Vonage*

Năm lớp em tóm tắt:

| Lớp | Vai trò |
|-----|---------|
| Client app (mobile/web) | Khởi tạo `CUSTOM_AUTH`, mở `check_url` qua mạng di động, gửi mã xác minh về Cognito |
| Amazon Cognito User Pool | Điều phối challenge flow, phát JWT khi thành công |
| 3 Lambda triggers | Define Auth Challenge, Create Auth Challenge (gọi Vonage), Verify Auth Challenge |
| Vonage APIs | Identity Insights + Verify + Fraud Defender |
| Mobile Network Operators | Xác minh cấp SIM qua CAMARA/Open Gateway |

![Sequence diagram luồng đăng nhập với SIM-swap pre-check và Silent Auth](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2026/06/09/ARCHBLOG-1533-2.png)

*Hình 2 — Luồng đăng nhập qua CUSTOM_AUTH*

Luồng 6 bước em ghi lại:

1. Client gọi `InitiateAuth` với `CUSTOM_AUTH`, truyền số điện thoại.
2. Lambda **Define Auth Challenge** → Cognito phát `CUSTOM_CHALLENGE`.
3. Lambda **Create Auth Challenge** → gọi Identity Insights → nếu pass, gọi Verify Silent Auth → trả `check_url` cho client.
4. Client mở HTTPS tới `check_url` → redirect qua mạng nhà mạng → nhận mã xác minh.
5. Client gọi `RespondToAuthChallenge` với mã.
6. Lambda **Verify Auth Challenge** → xác nhận với Vonage → Cognito cấp session token.

Điểm em đánh giá cao: **không cần đổi user pool hay app client hiện có** — plug vào `CUSTOM_AUTH`, deploy bằng `sam deploy`. Có thể rollout từng phase: bắt đầu journey rủi ro cao (đổi mật khẩu, giao dịch lớn), mở rộng dần sang login hàng ngày.

## Xác thực theo mức rủi ro từng journey

| Journey | Mức rủi ro | Workflow Vonage (tóm tắt) |
|---------|------------|---------------------------|
| Đăng ký tài khoản mới | CRITICAL | Lọc số không mobile + Subscriber Match KYC → Silent Auth zero-tap |
| Đăng nhập hàng ngày | MEDIUM | Kiểm tra SIM swap + thiết bị → Silent Auth thụ động, step-up khi có tín hiệu bất thường |
| Khôi phục mật khẩu, đổi profile/2FA | HIGH | SIM swap hard-check + Subscriber Match → bắt buộc Silent Auth |
| Giao dịch giá trị cao | CRITICAL | Full signal stack → Silent Auth + challenge phụ nếu rủi ro cao |

Hành động rủi ro thấp (xem thông tin tài khoản, duyệt nội dung) **không thêm friction**. Hành động nhạy cảm kích hoạt full stack — **policy-driven**, cấu hình theo từng journey.

## Triển khai và tuân thủ em ghi chú

**Prerequisites:** AWS account (Cognito, Lambda, Secrets Manager, CloudWatch, WAF), Cognito user pool, tài khoản Vonage API, AWS SAM CLI, SDK Silent Auth trên mobile.

**Bảo mật (Well-Architected Security Pillar):**
- IAM least privilege cho từng Lambda.
- TLS 1.2+; CloudWatch log + CloudTrail audit Cognito API.
- AWS WAF rate-limit trước endpoint auth chống brute-force.
- Credential Vonage lưu **AWS Secrets Manager**.

**Privacy:** PII không rời khỏi nhà mạng — Subscriber Match chỉ trả **match score**; Silent Auth không trao đổi PII, **cellular session là credential**. Hỗ trợ GDPR, PSD2/SCA, HIPAA, DORA, CCPA theo bài gốc.

**Kết quả thực tế — Lydia Solutions (fintech châu Âu):** triển khai tháng 10/2024; giảm latency tới **50%** so với auth cũ; conversion cải thiện **2–8,5%** so với chỉ SMS ở các deployment khác; latency journey auth giảm **50–75%**.

## Liên hệ với dự án thực tập

Law-Chatbot dùng Cognito cho auth web app — em chủ yếu cấu hình user pool, app client và JWT validation trên FastAPI. Bài Vonage mở rộng tầm nhìn: auth không chỉ là “login thành công”, mà là **quyết định rủi ro theo ngữ cảnh**.

Nếu sau này chatbot pháp luật có tính năng **tra cứu hồ sơ cá nhân** hoặc **tư vấn theo tài khoản định danh**, em nghĩ nên xem xét:
- pre-check SIM swap trước session nhạy cảm;
- Silent Auth cho mobile app thay vì SMS OTP thuần;
- WAF + rate limit trước API auth — pattern em chưa làm sâu trong kỳ thực tập.

Bài cũng nhắc em **chi phí ẩn của OTP**: không chỉ fraud mà cả user drop-off và helpdesk — metric cần đo khi đánh giá giải pháp auth.

## Kết luận

Thay vì áp cùng một lớp xác minh cho mọi session, doanh nghiệp có thể dùng **tín hiệu mạng real-time** để quyết định: xác thực im lặng khi an toàn, step-up khi có dấu hiệu rủi ro, chặn khi phát hiện fraud.

Stack Vonage (Identity Insights + Verify + Fraud Defender) + Cognito `CUSTOM_AUTH` là ví dụ **bảo mật và UX không loại trừ nhau** — triển khai với 3 Lambda, có thể bắt đầu từ journey rủi ro cao rồi mở rộng. Kiến thức hữu ích cho em khi thiết kế hệ thống có Cognito và user mobile-first sau khi tốt nghiệp.

### Reference

- [Reducing SMS OTP fraud with Vonage network-powered solutions and Amazon Cognito](https://aws.amazon.com/blogs/architecture/reducing-sms-otp-fraud-with-vonage-network-powered-solutions-and-amazon-cognito/)
- [Bài chia sẻ trên Facebook (AWS Study Group FCAJ)](https://www.facebook.com/groups/awsstudygroupfcj/posts/2240609806703979/)
