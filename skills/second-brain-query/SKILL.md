---
name: second-brain-query
description: Answer questions from Odin's second brain with provenance. Use to retrieve and synthesize from wiki/ and memory/, citing the source path for each claim.
---

# second-brain-query

Purpose: answer questions from Odin's second brain with provenance.

## Flow

1. Search `wiki/` first.
2. Follow cited raw source paths when an answer depends on a specific claim.
3. Search `raw/` when wiki coverage is missing or stale.
4. Search `memory/durable` for approved durable facts.
5. Search `memory/candidates` only as unapproved hints.
6. Answer with links or paths to the supporting wiki and raw files.

## Rules

- Raw files are the source of truth.
- Wiki pages are compiled understanding.
- Memory candidates are not durable truth.
- If wiki and raw disagree, trust raw and flag the wiki page for repair.
- Do not update durable memory during query work.
