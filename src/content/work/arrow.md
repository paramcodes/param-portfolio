---
title: Arrow
summary: desktop code editor — tauri v2, react 19, ai completions
year: 2026
tags: [tauri, react, rust, desktop, ai]
repo: https://github.com/paramcodes/arrow
featured: true
order: 4
---

A desktop code editor built with Tauri v2, React 19 and Vite, with AI completions wired in. Native binary, web-rendered UI — the Tauri/Rust shell keeps memory use sane where Electron projects sprawl.

The interesting problems are the editor-specific ones: virtualized buffers that stay responsive on large files, completion UX that never blocks typing, and a Rust-side command layer for file system work. Actively developed; this is the current showpiece.
