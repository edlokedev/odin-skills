# reflect — reference

Detailed taxonomy, autonomy rules, and templates for the `reflect` skill. SKILL.md is the entry point; read this when you need the full picture.

## Finding taxonomy

Classify each observation into one of these. The label drives the kind of fix.

1. **Repeated mistake** — the same error or wrong turn shows up across sessions (e.g. running `bun install` without `HUSKY=0` on the mount, editing a generated file by hand). Fix: a durable rule (memory candidate → propose for durable) or an instruction line, or a deterministic gate.
2. **Wasted / incorrect tool calls** — the agent searched the same thing repeatedly, fetched a client-rendered page with the wrong tool, retried a blocked action. Fix: a how-to note or a skill that encodes the right path.
3. **Missing up-front context** — the agent had to discover something it should have known on entry (a project's deploy target, a shell quirk, a naming convention). Fix: add to the project's `AGENTS.md`/`status.md` (propose), or a memory candidate.
4. **Stalled or looping work** — work that took far longer than it should have, or went in circles. Fix: a clearer workflow, a checklist, or a verification step.
5. **Discovered preference** — an operator preference learned mid-task that wasn't written down. Fix: memory candidate; propose for durable if stable.

For each finding record: a one-line description, the **evidence path(s)**, the class, whether it's project-specific or cross-project, and the proposed fix with its autonomy class.

## Autonomy classes (full)

**Auto-apply (safe — inert, additive, reversible):**

- `memory/candidates/*` — newly-learned facts, preferences, constraints (one fact each).
- `.odin/runs/<date>-reflect-report.md` — the run report (flat file; a network-drive workspace can't create a `<date>-reflect/` subfolder).
- `.odin/runs/reflect-log.md` — one appended line per run.
- `wiki/*` notes that only compile existing cited evidence.

**Propose only (risky — changes behaviour broadly or hard to undo):**

- `AGENTS.md` / instruction edits → always via `AGENTS.md` then `sync-instructions.sh`.
- New or edited skills under `skills/` (and their projection via `sync-skills.sh`).
- New/changed scripts, pre-commit or other hooks, anything that runs automatically.
- `memory/durable/*`.
- Deletes, dependency adds, git-remote changes, destructive commands, service installs.

Rationale for the line: candidate memory and reports do nothing until a human reads them. A skill or an instruction line changes how every future agent behaves the moment it lands, so it needs a human in the loop — this is the whole point of Odin staying reversible and inspectable.

## Report template

Write to `.odin/runs/<YYYY-MM-DD>-reflect-report.md`:

```markdown
# Reflect report — <YYYY-MM-DD>

- **Window:** <e.g. last 24h>
- **Evidence reviewed:** <paths/sources, counts>
- **Confidence:** <low/med/high, with why>

## Findings
| # | Class | Scope | Finding | Evidence | Proposed fix | Autonomy |
|---|-------|-------|---------|----------|--------------|----------|
| 1 | repeated-mistake | project:gymmie | ... | memory/sessions/... | rule X | applied / proposed |

## Applied (safe)
- memory/candidates/<file> — <one line>
- ...

## Proposed (needs approval)
### P1 — <title>  [instruction edit | new skill | script/gate | durable memory]
- **Why:** ...
- **Exact change:** <ready-to-paste diff or full file content>
- **How to apply:** <command, e.g. edit AGENTS.md then `bash .odin/scripts/sync-instructions.sh`>

## Watched, no action
- ...
```

## reflect-log.md line format

Append one line per run to `.odin/runs/reflect-log.md`:

```
<YYYY-MM-DD HH:MM> | window=<24h> | findings=<n> | applied=<n> | proposed=<n> | report=.odin/runs/<date>-reflect-report.md
```

## Notes on evidence sources

- `session_info` only sees currently-visible sessions, not an archive. The durable signal is `memory/sessions/` handoffs — which is why AGENTS.md asks agents to leave a handoff. Treat a thin session record as its own finding ("learning was lost; encourage handoffs").
- Repo `git log`/diffs show what changed but not why it was hard; pair them with the matching session/run log.
- Always prefer Git Bash for commands (`bash ...`); the workspace is Windows with Mountain Duck mounts.
- If the workspace is mounted as a network drive (e.g. `N:`), the Linux shell can't see it at all — only outputs/uploads/.claude are mounted. Gather evidence with the file tools (Glob/Grep/Read) and skip the bash `git log`/`find`/`.odin` steps. Creating the report's `<date>-reflect/` subfolder also fails there, which is why the report path is a flat file.
- The previous report's "Proposed" section is itself evidence: confirm each prior proposal's current status against the tree and tabulate it, so applied work isn't re-investigated and open items don't get lost. Note that `.claude/skills/` is a harness-protected path the file tools can't write — when a skill edit can't reach that projection, mirror it to `skills/` (canonical) + `.agents/skills/` and have the operator run `sync-skills.sh` to refresh `.claude/skills/`.
