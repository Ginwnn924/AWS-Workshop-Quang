---
title: "RAG Pipeline"
date: 2026-08-11
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---
## Mục tiêu

Cấu hình và kiểm thử toàn bộ RAG pipeline: từ embedding câu hỏi, truy xuất vector, reranking đến sinh câu trả lời bằng LLM.

## Kiến trúc RAG Pipeline

```text
Query → Embedding → pgvector Search (Top-K) → Reranker (Top-N) → Prompt Builder → LLM → Answer
```

## Các thành phần chi tiết

### 1. Embedding Model

Hệ thống sử dụng model embedding chuyên biệt cho tiếng Việt:

| Cấu hình | Giá trị |
| --- | --- |
| Model | `AITeamVN/Vietnamese_Embedding` |
| Dimension | 768 |
| Framework | SentenceTransformers |
| Fallback | Amazon Bedrock Titan Embeddings |

```python
# src/rag_core/embeddings.py
from sentence_transformers import SentenceTransformer

model = SentenceTransformer("AITeamVN/Vietnamese_Embedding")
embedding = model.encode("Điều 15 Luật Dân sự quy định gì?")
```

### 2. Retriever (Vector Search)

Retriever thực hiện cosine similarity search trên pgvector:

| Parameter | Giá trị mặc định | Mô tả |
| --- | --- | --- |
| `TOP_K` | 10 | Số lượng chunks trả về từ vector search |
| Distance metric | Cosine | `<=>` operator trong pgvector |
| Filter | `status = 'active'` | Chỉ search trên documents chưa bị soft-delete |

### 3. Reranker

Cross-encoder reranker xếp hạng lại kết quả retrieval để tăng precision:

| Cấu hình | Giá trị |
| --- | --- |
| Enabled | `RERANK_ENABLED=true` |
| Model | Cross-encoder model |
| Top-N sau rerank | 5 |

Reranker tính toán relevance score giữa query và mỗi chunk, sau đó chỉ giữ lại top-N chunk có score cao nhất để đưa vào prompt.

### 4. Prompt Construction

Template prompt được thiết kế để LLM trả lời dựa trên ngữ cảnh pháp luật:

```python
PROMPT_TEMPLATE = """
Bạn là chuyên gia tư vấn pháp luật Việt Nam. Dựa trên các điều khoản pháp luật sau đây,
hãy trả lời câu hỏi của người dùng một cách chính xác và dễ hiểu.

NGỮ CẢNH PHÁP LUẬT:
{context}

CÂU HỎI: {question}

Yêu cầu:
- Trả lời dựa trên ngữ cảnh được cung cấp
- Trích dẫn điều khoản cụ thể khi có thể
- Nếu ngữ cảnh không đủ, nêu rõ giới hạn của câu trả lời
"""
```

### 5. LLM Generation

Hỗ trợ dual provider với khả năng chuyển đổi qua biến môi trường:

| Provider | Model | Cấu hình |
| --- | --- | --- |
| Google | Gemini 2.5 Flash | `LLM_PROVIDER=gemini`, `GEMINI_API_KEY=...` |
| AWS | Bedrock (Claude 3 / Llama 3) | `LLM_PROVIDER=bedrock`, `AWS_REGION=...` |

### 6. Response Format

Mỗi response bao gồm:

```json
{
  "answer": "Theo Điều 15 Bộ luật Dân sự 2015...",
  "sources": [
    {"document": "Bộ luật Dân sự 2015", "chunk": "Điều 15..."}
  ],
  "timings_ms": {
    "embedding": 45,
    "db_search": 120,
    "rerank": 85,
    "llm_generation": 2100,
    "total": 2350
  }
}
```

## Kiểm thử Pipeline

```bash
# Test endpoint đơn giản
curl -X POST http://localhost:8000/ask \
  -H "Content-Type: application/json" \
  -d '{"question": "Quyền sở hữu tài sản được quy định như thế nào?"}'
```

## Tinh chỉnh hiệu năng

| Parameter | Ảnh hưởng | Khuyến nghị |
| --- | --- | --- |
| `CHUNK_SIZE` | Chunk lớn → context đầy đủ hơn, chunk nhỏ → precise hơn | 512 characters |
| `CHUNK_OVERLAP` | Overlap cao → giảm mất ngữ cảnh ở biên chunk | 50 characters |
| `TOP_K` | K lớn → recall cao, latency cao hơn | 10 |
| `RERANK_ENABLED` | Bật → precision tăng, thêm ~100ms latency | true |
