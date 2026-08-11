---
title: "Prerequisites"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Prerequisites — Các bước chuẩn bị

Trước khi setup các dịch vụ AWS ở các mục sau, cần chuẩn bị môi trường và quyền truy cập tối thiểu như dưới đây.

## 1. Tài khoản và region

- Tài khoản AWS đang hoạt động
- Region khuyến nghị: **ap-southeast-1**
- Bật MFA cho IAM user / root nếu chưa bật
- Theo dõi chi phí bằng AWS Budgets khi thực hành lab

## 2. Máy chủ EC2

- Instance type demo: **t3a.small**
- Hệ điều hành: Amazon Linux hoặc Ubuntu
- Ổ đĩa đủ chỗ cài Docker image
- Subnet có thể kết nối tới RDS và Bedrock, hoặc có egress Internet khi cần

## 3. Công cụ trên EC2

Cài đặt và kiểm tra:

sudo yum install -y docker git
# hoặc apt tương đương trên Ubuntu
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
# đăng nhập lại session sau khi thêm group docker
docker --version
git --version

## 4. Clone codebase

git clone <your-repo-url> vietnamese-legal-llmops
cd vietnamese-legal-llmops
cp .env.sample .env

{{% notice warning %}}
Không commit file .env chứa mật khẩu RDS, API key hoặc Access Key vào Git.
{{% /notice %}}

## 5. Security Group

| Hướng | Port | Ghi chú |
| --- | --- | --- |
| Inbound EC2 | **8501** | Streamlit UI — chỉ mở từ IP của bạn hoặc ALB |
| Inbound EC2 | **8000** | FastAPI — tùy chọn, thường chỉ nội bộ Docker |
| Inbound RDS | **5432** | Chỉ nhận từ Security Group của EC2 |

## 6. IAM và LLM

- Ưu tiên **IAM Instance Role** cho EC2 để gọi Bedrock / S3
- Tránh gắn Access Key dài hạn trên máy nếu có thể dùng role
- Chọn một trong hai hướng LLM:
  - **Bedrock** — bật model access trong region
  - **Gemini** — có GEMINI_API_KEY trong .env cho môi trường dev

## 7. Checklist trước mục 5.3

- [ ] Đăng nhập AWS Console / CLI được
- [ ] EC2 sẵn sàng, SSH hoặc Session Manager được
- [ ] Docker và Git chạy được trên EC2
- [ ] Repo vietnamese-legal-llmops đã clone, có file .env
- [ ] Security Group và IAM/role đã cấu hình tối thiểu
- [ ] Đã chọn Bedrock hoặc Gemini

Sang mục tiếp theo: tạo **S3 bucket** và upload data.
