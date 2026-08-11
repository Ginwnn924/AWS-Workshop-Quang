---
title: "Monitoring và Evaluation"
date: 2026-08-11
weight: 6
chapter: false
pre: " <b> 5.4.6. </b> "
---
## Mục tiêu

Thiết lập hệ thống giám sát với CloudWatch, structured logging và module đánh giá chất lượng RAG pipeline.

## Structured Logging

Hệ thống sử dụng structured logging (`src/monitoring/logging_config.py`) với format JSON để dễ dàng query trong CloudWatch Log Insights:

```json
{
  "timestamp": "2026-08-11T10:30:00Z",
  "level": "INFO",
  "service": "rag-api",
  "event": "query_processed",
  "question_length": 45,
  "timings_ms": {
    "embedding": 42,
    "db_search": 115,
    "rerank": 78,
    "llm_generation": 2050,
    "total": 2285
  },
  "chunks_retrieved": 10,
  "chunks_after_rerank": 5
}
```

## CloudWatch Configuration

### Log Groups

| Log Group | Source | Retention |
| --- | --- | --- |
| `/legal-rag/api` | FastAPI container logs | 30 days |
| `/legal-rag/streamlit` | Streamlit container logs | 14 days |
| `/legal-rag/lambda-ingestion` | Lambda function logs | 7 days |

### Log Insights Queries

**Latency percentiles:**

```
fields @timestamp, timings_ms.total
| filter event = "query_processed"
| stats avg(timings_ms.total) as avg_ms,
        pct(timings_ms.total, 50) as p50,
        pct(timings_ms.total, 95) as p95,
        pct(timings_ms.total, 99) as p99
  by bin(1h)
```

**Error rate:**

```
fields @timestamp, @message
| filter level = "ERROR"
| stats count() as error_count by bin(5m)
```

## Evaluation Module

### Offline Metrics

Module `src/evaluation/` đánh giá chất lượng retrieval trên test set:

| Metric | Công thức | Ý nghĩa |
| --- | --- | --- |
| Recall@k | Relevant docs retrieved / Total relevant docs | Tỷ lệ tài liệu liên quan được tìm thấy |
| MRR | 1/rank of first relevant result | Vị trí trung bình của kết quả đầu tiên đúng |

### Chạy Evaluation

```bash
python scripts/eval.py \
  --test-set data_demo/test_questions.json \
  --top-k 10 \
  --output results/eval_results.json
```

### Benchmark QA Latency

```bash
python scripts/benchmark_qa.py \
  --questions data_demo/benchmark_questions.txt \
  --iterations 50

# Output:
# p50: 2.1s, p95: 3.8s, max: 5.2s
```

## User Feedback

Hệ thống thu thập feedback từ người dùng qua `src/monitoring/feedback_store.py`:

| Field | Type | Mô tả |
| --- | --- | --- |
| `session_id` | String | Phiên hội thoại |
| `message_id` | String | Message được đánh giá |
| `rating` | Integer (1-5) | Điểm đánh giá |
| `comment` | String | Nhận xét bổ sung |

Feedback được lưu vào bảng `app_feedback` trong PostgreSQL, phục vụ phân tích chất lượng và cải thiện prompt/retrieval.
