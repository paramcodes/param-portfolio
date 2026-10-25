---
title: rag that survives production
description: what breaks when a rag demo meets real traffic, and the pipeline decisions that keep it standing.
pubDate: 2026-11-05
---

Every RAG demo works. The interesting question is what happens on week three, when the documents stop being clean PDFs and the traffic stops being you.

This post walks through the decisions in Atlas RAG API and why each one exists: hierarchical indexing so retrieval holds up when the corpus gets messy, reciprocal rank fusion because no single retriever wins, reranking as the last line of defense, and Prometheus metrics on retrieval quality — not just uptime — because a RAG system that answers confidently and wrongly is worse than one that 502s.

Full write-up coming November 2026.
