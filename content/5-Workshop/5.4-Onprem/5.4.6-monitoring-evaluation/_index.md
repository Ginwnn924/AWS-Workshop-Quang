---
title: "Monitoring & Evaluation"
date: 2026-08-11
weight: 6
chapter: false
pre: " <b> 5.4.6. </b> "
---
## Objective

Set up monitoring with CloudWatch structured logging and RAG quality evaluation module.

## Steps

1. Configure structured JSON logging for CloudWatch Log Insights
2. Set up log groups with appropriate retention policies
3. Create Log Insights queries for latency percentiles and error rates
4. Run offline evaluation (Recall@k, MRR) on test sets
5. Benchmark QA latency (p50, p95, max)
6. Collect user feedback for quality improvement
