---
title: Junior Doctor
summary: patient records to discharge summaries, with conflict checks
year: 2025
tags: [ai-agents, python, gemini, pymupdf, pydantic]
repo: https://github.com/paramcodes/junior-doctor
featured: true
order: 2
---

An AI agent that turns scanned patient records into structured discharge summaries. Scanned pages go through OCR with a manual fallback path, PyMuPDF extracts whatever text layer exists, and the agent organizes the result into validated, typed output — every field checked by Pydantic schemas before it's allowed to leave the pipeline.

The detail that matters most in a medical context is refusal to guess: the agent detects medication conflicts between what the chart says and what the summary claims, and surfaces them instead of papering over them. Built with the Gemini API, designed so a clinician reviews a draft, not a mystery.
