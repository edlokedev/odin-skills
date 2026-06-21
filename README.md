# Odin Skills

A platform-agnostic bundle of **57 agent skills** — design, code review, planning, TDD,
plan-grilling, research, and workspace utilities — packaged so any coding agent can use
them locally or on a cloud/server runtime.

- **Canonical instructions:** [AGENTS.md](AGENTS.md)
- **Skills:** [`skills/<name>/SKILL.md`](skills/)
- **Attribution / licenses:** [NOTICE.md](NOTICE.md)

Each skill is a folder under `skills/` with a `SKILL.md` whose front matter declares a
`name` and a `description` (when to use it). Some skills bundle extra files
(`REFERENCE.md`, scripts, templates).

---

## Claude Code

Install once as a plugin marketplace — works in local Claude Code **and** in
cloud/scheduled runs because the source is a public git repo (no auth needed):

```bash
claude plugin marketplace add edlokedev/odin-skills
claude plugin install odin-skills@odin-skills
```

Skills are then auto-discovered and invocable via the `Skill` tool or `/<skill-name>`.
Update later with:

```bash
claude plugin marketplace update odin-skills
```

## Codex (OpenAI)

Codex reads `AGENTS.md`. Make the repo part of the agent's working tree — e.g. in a Codex
**environment setup** step, clone it:

```bash
git clone https://github.com/edlokedev/odin-skills ~/odin-skills
```

Then point Codex at `~/odin-skills/AGENTS.md`, or copy `skills/` into the project you want
the skills available in. Codex opens `skills/<name>/SKILL.md` on demand based on the
descriptions.

## Gemini CLI / other agents

Clone the repo and start from [GEMINI.md](GEMINI.md) / [AGENTS.md](AGENTS.md). Treat
`skills/` as the skill library and open a skill's `SKILL.md` when a task matches its
description.

---

## Source of truth

These skills are maintained in the private Odin workspace under `skills/` and published
here as a read-only mirror. Open an issue for corrections or removals.
