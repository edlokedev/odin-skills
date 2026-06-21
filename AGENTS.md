# Odin Skills — Agent Operating Guide

This repository is a **platform-agnostic bundle of agent skills**. It is designed to be
consumed by any coding agent — Claude Code, Codex (OpenAI), Gemini CLI, and others —
on a local machine or on a cloud/server runtime.

`AGENTS.md` is the canonical instruction source. `CLAUDE.md` and `GEMINI.md` are generated
adapters that point back here.

## What is here

- `skills/<name>/SKILL.md` — one folder per skill. The `SKILL.md` front matter has a
  `name` and a `description` that says when to use it. Some skills bundle extra files
  (`REFERENCE.md`, scripts, templates) in the same folder.

There are no secrets, no project data, and no runtime state in this repo — only skills.

## How to use a skill

1. When a task matches a skill's `description`, open that skill's `skills/<name>/SKILL.md`
   and follow it.
2. Read any bundled files the skill references (e.g. `REFERENCE.md`) before acting.
3. Skills are guidance, not automation. Apply judgement; do not force a skill when a
   direct answer or edit is enough.

## Platform notes

- **Claude Code** — installed as a plugin; skills in `skills/` are auto-discovered and
  invocable via the `Skill` tool / `/<skill-name>`. See `README.md` for install.
- **Codex (OpenAI)** — reads this `AGENTS.md`. Clone the repo into the environment and
  read `skills/<name>/SKILL.md` on demand based on the descriptions above.
- **Gemini CLI** — reads `GEMINI.md`, which points here.
- **Other agents** — start from this file and treat `skills/` as the skill library.

## Attribution

Many skills originate from third parties (see `NOTICE.md`). Their original licenses are
preserved inside the relevant skill folders. Keep attribution intact when redistributing.
