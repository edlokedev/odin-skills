---
name: second-brain-ingest
description: Turn raw material into compiled wiki pages following the raw -> wiki -> schema pattern. Use when processing the raw-ingest queue; cite every raw source path.
---

# second-brain-ingest

Purpose: turn raw material into compiled wiki pages.

This follows the raw -> wiki -> schema pattern:

- `raw/` is immutable source material.
- `wiki/` is LLM-compiled understanding.
- `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, manifests, and templates are schema/instruction layers that tell agents how to read and maintain the system.

## Default Flow

1. Run raw ingest from the Odin root:

```bash
bash .odin/scripts/raw-ingest.sh
# fallback if Git Bash is unavailable
bun run .odin/scripts/raw-ingest.js
```

2. Read `.odin/raw-ingest-queue.md`.
3. For each useful queued source, read the original raw file.
4. Create or update focused pages under `wiki/concepts`, `wiki/entities`, `wiki/projects`, or `wiki/synthesis`.
5. Cite raw source paths in every generated wiki page.
6. Create `memory/candidates` only for facts, preferences, decisions, or constraints that are likely to be reused.
7. Leave durable memory untouched unless explicitly approved.

## Rules

- `raw/` is source material.
- `wiki/` is compiled understanding.
- Do not alter raw files.
- Cite source filenames in wiki pages.
- Prefer small, linked pages over giant summaries.
- Do not wait for the operator to manually sort raw files.
- If a raw file is too large or unclear, leave it queued and add a brief note to `.odin/raw-ingest-queue.md`.
- Do not create memory directly from raw without first checking whether a wiki page should exist.
