---
title: "API Deployment với Docker"
date: 2026-08-11
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---
## Mục tiêu

Build Docker image cho ứng dụng, cấu hình Docker Compose và triển khai trên EC2 instance.

## Cấu trúc Docker

### Dockerfile

Ứng dụng sử dụng Python 3.11-slim base image, cài đặt CPU-only PyTorch để giảm image size:

```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements-deploy.txt .
RUN pip install --no-cache-dir -r requirements-deploy.txt

COPY . .
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
```

### Entrypoint script

`entrypoint.sh` hỗ trợ ba mode chạy qua biến `APP_MODE`:

| Mode | Lệnh | Port |
| --- | --- | --- |
| `api` | `uvicorn src.api.app:app --host 0.0.0.0 --port 8000` | 8000 |
| `streamlit` | `streamlit run streamlit_app.py --server.port 8501` | 8501 |
| `chainlit` | `chainlit run app.py --port 8000` | 8000 |

### Docker Compose

```yaml
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - APP_MODE=api
    env_file:
      - .env
    restart: unless-stopped

  streamlit:
    build: .
    ports:
      - "8501:8501"
    environment:
      - APP_MODE=streamlit
    env_file:
      - .env
    depends_on:
      - api
    restart: unless-stopped
```

## Triển khai trên EC2

### 1. Tạo EC2 Instance

| Cấu hình | Giá trị |
| --- | --- |
| AMI | Amazon Linux 2023 |
| Instance type | `t3.medium` (2 vCPU, 4 GB RAM) |
| Storage | 30 GB gp3 |
| Security group | `sg-legal-app` |
| Key pair | Tạo mới hoặc sử dụng existing |

### 2. Cài đặt Docker trên EC2

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

# Cài Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### 3. Clone repository và cấu hình

```bash
git clone <repository-url> ~/legal-rag
cd ~/legal-rag
cp .env.sample .env
# Chỉnh sửa .env với các giá trị từ CloudFormation Outputs
nano .env
```

### 4. Build và khởi chạy

```bash
cd deploy/
docker-compose up -d --build

# Kiểm tra logs
docker-compose logs -f api
docker-compose logs -f streamlit
```

### 5. Xác minh

```bash
# Health check API
curl http://localhost:8000/health

# Test chat endpoint
curl -X POST http://localhost:8000/ask \
  -H "Content-Type: application/json" \
  -d '{"question": "Luật lao động quy định gì về thời gian nghỉ phép?"}'

# Truy cập Streamlit UI
# http://<EC2-Public-IP>:8501
```

## Initialize Application Database

Trước khi sử dụng các tính năng admin và user management, cần khởi tạo application tables:

```bash
docker-compose exec api python scripts/init_app_db.py
```

Lệnh này tạo các bảng `app_users`, `app_chat_sessions`, `app_chat_messages`, `app_feedback` trong PostgreSQL.
