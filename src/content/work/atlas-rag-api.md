---
title: Atlas RAG API
summary: production rag — raptor indexing, rrf fusion, prometheus & grafana
year: 2025
tags: [rag, fastify, pgvector, docker, observability]
repo: https://github.com/paramcodes/rag-agent-api
featured: true
order: 1
---

A retrieval-augmented generation service built to survive real traffic, not just demos. Documents are indexed with RAPTOR-style hierarchical clustering (UMAP + HDBSCAN) so retrieval works at both detail and summary level, and queries are answered by fusing dense vector search with BM25 through reciprocal rank fusion, then reranked with Jina before the model ever sees a token.

The unglamorous parts are where the work went: pgvector for storage, Fastify for a tight HTTP layer, Docker for reproducible deploys, and Prometheus metrics piped into Grafana so latency and retrieval quality are observable in production. Answers ship with citations — every claim traces back to a source chunk.
