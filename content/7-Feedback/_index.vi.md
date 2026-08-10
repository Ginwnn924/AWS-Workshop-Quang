---
title: "Chia sẻ, đóng góp ý kiến"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 7. </b> "
---


### Đánh giá chung

**1. Môi trường làm việc**

Môi trường FCAJ thân thiện và cởi mở. Các bạn thực tập và mentor sẵn sàng trao đổi trên nhóm chat khi gặp lỗi AWS hoặc vướng lab workshop — ví dụ tuần 2 khi tôi không SSH được vào EC2 vì security group sai, bạn trong nhóm chỉ tôi dùng Reachability Analyzer trong vài phút. Không gian học tập (online/offline tùy buổi) giúp tập trung làm lab VPC Endpoint và dự án cá nhân. Tôi mong có thêm buổi **lightning talk** ngắn (5 phút/dự án) để học hỏi lẫn nhau ngoài mentor. Nên có kênh **FAQ theo tuần** (lỗi CloudFormation, IAM deny) để search lại thay vì hỏi lặp.

**2. Sự hỗ trợ của mentor / team admin**

Mentor hướng dẫn rõ ràng khi tôi chưa quen Bedrock model access hay cấu hình RDS security group. Thay vì đưa đáp án ngay, mentor gợi ý hướng đọc tài liệu AWS — giúp tôi tự debug và nhớ lâu hơn. Team admin hỗ trợ kịp thời các thủ tục, deadline nộp báo cáo và template Hugo. Tôi đánh giá cao việc được phép **thử-sai** trên AWS account thực tập với budget alert — tuần 4 tôi lỡ để NAT Gateway chạy qua cuối tuần, cảnh báo budget là bài học thực tế về chi phí cloud.

**3. Sự phù hợp giữa công việc và chuyên ngành học**

Dự án Legal RAG Chatbot rất phù hợp ngành CNTT: kết hợp backend (Python/FastAPI), AI (RAG, LLM), cloud (AWS) và viết báo cáo kỹ thuật. Phần workshop S3/VPC bổ sung kiến thức mạng — trước đây ở trường tôi ít được thực hành VPC Endpoint. Chương trình mở rộng đúng hướng so với giảng đường.

**4. Cơ hội học hỏi & phát triển kỹ năng**

Tôi học được nhiều kỹ năng mới:

* Triển khai và cấu hình dịch vụ AWS (EC2, S3, RDS, Lambda, Cognito, Bedrock...).
* Thiết kế pipeline RAG end-to-end, không chỉ gọi API chatbot có sẵn.
* Viết Infrastructure as Code cơ bản với CloudFormation.
* Kỹ năng mềm: báo cáo worklog hàng tuần, demo sản phẩm, nhận feedback.

Tài liệu [Cloud Journey](https://cloudjourney.awsstudygroup.com/) và workshop nội bộ là nguồn tham khảo quan trọng suốt 8 tuần. Các lab AWS Study Group (IAM, VPC, RDS, S3, Cognito, Lambda) giúp setup nhanh hơn so với đọc docs AWS thuần tiếng Anh.

**5. Văn hóa & tinh thần đồng đội**

Văn hóa FCAJ tích cực: mọi người tôn trọng lẫn nhau dù khác trường hoặc đề tài. Khi gần deadline workshop cleanup, các bạn nhắc nhau xóa resource tránh phát sinh chi phí — thể hiện tinh thần **cloud cost awareness** rất thực tế. Tuần 3 viết Proposal, nhóm cùng review sơ đồ kiến trúc và phát hiện thiếu route VPC Endpoint trước khi deploy. Tôi cảm thấy được coi là thành viên, không chỉ "người đi học cho xong".

**6. Chính sách / phúc lợi cho thực tập sinh**

Chương trình cung cấp AWS account lab, mentor và lộ trình rõ ràng 8 tuần. Thời gian linh hoạt một phần khi có deadline học phần ở trường. Việc được tham gia cộng đồng AWS Study Group là điểm cộng lớn cho sự nghiệp sau này. Template Hugo báo cáo song ngữ (VI/EN) cũng giúp rèn kỹ năng viết tài liệu kỹ thuật — hữu ích cho portfolio và phỏng vấn.

### Một số câu hỏi phản hồi

**Điều bạn hài lòng nhất trong thời gian thực tập?**

Được làm dự án **thật** (legal chatbot RAG trên AWS) thay vì chỉ làm slide lý thuyết. Khi demo chatbot trả lời đúng câu hỏi về Luật Đất đai kèm trích dẫn chunk nguồn, tôi thấy rõ giá trị của 8 tuần nỗ lực. Deploy site Hugo báo cáo đầy đủ worklog + proposal + workshop cũng là milestone portfolio cụ thể.

**Điều bạn nghĩ chương trình cần cải thiện cho thực tập sinh sau?**

* Thêm buổi **office hour** cố định cho nhóm Bedrock/AI — nhiều bạn vướng model access và quota.
* Checklist **cost cleanup** cuối tuần (RDS, EC2, NAT Gateway, Elastic IP) gửi tự động — tránh phát sinh chi phí ngoài ý muốn.
* Template proposal/worklog có thêm **ví dụ đề tài AI/GenAI** bên cạnh IoT/serverless hiện tại.
* Video hướng dẫn ngắn setup Hugo + deploy GitHub Pages — giảm vướng ở tuần cuối nộp báo cáo.

**Nếu giới thiệu cho bạn bè, bạn có khuyên họ tham gia FCAJ không? Vì sao?**

**Có.** Lý do: lộ trình AWS thực hành rõ ràng, có mentor, có cộng đồng Study Group, và cơ hội build portfolio project đưa vào CV. Phù hợp sinh viên năm 3–4 muốn đi theo hướng cloud hoặc AI engineer.

### Đề xuất & mong muốn

* Tổ chức thêm **hackathon mini** cuối kỳ (1–2 ngày) để các team demo cross-review.
* Mở rộng tài liệu tiếng Việt cho Amazon Bedrock và pgvector trên RDS — hiện phần lớn docs AWS là tiếng Anh.
* Tôi mong được **tiếp tục đóng góp** vào repo open-source workshop hoặc blog AWS Study Group sau khi kết thúc thực tập.
* Góp ý khác: Nên có kênh Slack/Discord lưu trữ Q&A theo tuần — dễ search lại câu hỏi về lỗi CloudFormation, IAM policy thay vì hỏi lại nhiều lần.

---

Cảm ơn team FCAJ, mentor và AWS Study Group đã tạo môi trường học tập bổ ích trong suốt kỳ thực tập của tôi.
