---
name: reflect
description: Review recent Odin work for friction and turn it into improvements. Use when the user wants to improve how agents operate, run a retrospective, or when invoked on a schedule (e.g. nightly). Mines sessions, run logs, and repo activity; auto-applies safe additive changes and proposes behaviour-changing ones for approval.
argument-hint: "Optional: lookback window (e.g. '24h', '7d') or a focus area"
---

# reflect

Make Odin a little better every run. Look at how recent work actually went, find what slowed things down or went wrong, and feed the lessons back into memory, skills, and instructions.

This is Odin's continual-learning loop. It is meant to run unattended (a nightly schedule) and also on demand. It must stay reversible and inspectable: see [REFERENCE.md](REFERENCE.md) for the full finding taxonomy, the autonomy policy, and the report template.

## Scope of a run

Default lookback is **24h**, or **since the last `reflect` run** if that is longer. Honour an argument like `7d` or a focus area (e.g. `gymmie`) if given.

## Steps

1. **Gather evidence** from Odin's own durable record (do not invent a transcript store that does not exist):
   - `memory/sessions/*` — session summaries and handoffs in the window. These are the primary signal.
   - `.odin/runs/*` — run logs (`00-task.md`, `01-plan.md`, `02-review.md`, execution logs).
   - **Prior proposals:** read the previous `.odin/runs/<date>-reflect-report.md` "Proposed" section and check, against the current tree, which proposals were applied vs still open. Record the result as a short "Proposal resolution status" table in this run's report, and carry forward any still-open proposal instead of re-deriving it from scratch.
   - Root agent artifacts if present: `.codex-run.jsonl`, `.codex-err.txt`, `.codex-verdict.txt`.
   - Repository activity: for each active project in `.odin/project-registry.yaml`, `git log --since`, `git status`, recent diffs.
   - Live sessions via `session_info` (`list_sessions`, then `read_transcript`) when any are visible.
   - Recently modified workspace files: `find . -mtime -1 -not -path './.git/*'`.
   - **Shell reachability:** if the workspace is a Windows network drive (e.g. `N:`), the Linux shell cannot see it — only outputs/uploads/.claude are mounted. Gather all evidence with the file tools (Glob/Grep/Read); `git log`/`find`/`.odin/*.sh` via bash will not find the workspace. The Integrated Mountain Duck mount (`C:\Users\...\Mountain Duck\...\ODIN`, no drive letter) is likewise outside the Linux mount.

2. **Find friction.** Classify every observation using the taxonomy in [REFERENCE.md](REFERENCE.md): repeated mistakes, wasted/incorrect tool calls, missing context the agent needed up front, places work stalled or looped, and preferences that were discovered the hard way. Note whether each is project-specific or cross-project.

3. **Decide the fix** for each finding: a memory candidate, an instruction change, a new/edited skill, a script or gate, or "no action / watch". Map each to the autonomy classes below.

4. **Apply the safe changes** (see autonomy policy). Write a dated report to `.odin/runs/<date>-reflect-report.md` (a flat file — on a network-drive workspace no tool can create a `<date>-reflect/` subfolder) and append a one-line entry to `.odin/runs/reflect-log.md`.

5. **Stage the risky changes** as concrete, ready-to-apply proposals inside the report — exact diffs, exact file contents — so approval is one step, not a redesign.

6. **Summarise**: what you applied, what you are proposing and why, and what you watched but did not act on.

## Autonomy policy (auto-apply safe / propose risky)

**Auto-apply** — inert until a human reads them, fully additive and reversible:

- `memory/candidates/*` for newly-learned facts, preferences, constraints.
- The dated report under `.odin/runs/` and the `reflect-log.md` entry.
- New `wiki/` notes that only compile existing evidence (cite the source path).

**Propose only** (write into the report; do not apply) — anything that changes agent behaviour broadly or is hard to undo:

- Edits to `AGENTS.md` or any instruction file. (Instruction changes go through `AGENTS.md` + `sync-instructions.sh`, never by hand-editing `CLAUDE.md`/`GEMINI.md`.)
- New or edited **skills** — once projected they silently change every future agent.
- New/changed scripts, pre-commit hooks, or anything that runs automatically.
- Durable memory (`memory/durable/*`).
- Any delete, dependency add, git-remote change, or destructive command.

When unsure which class a change is in, treat it as risky and propose it.

## Guardrails

- Never write durable memory or edit instructions without approval (Odin "Ask First").
- Cite the evidence path for every finding. No finding without a source.
- Do not put secrets in the report, memory, or wiki.
- Keep memory candidates small and specific — one fact each, using `.odin/templates/memory-candidate.yaml`.
- If you find nothing worth acting on, say so. A short honest report beats invented work.
