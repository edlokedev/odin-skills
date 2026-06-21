---
name: self-verify
description: Check your own work before reporting it done. Use as the final step of any non-trivial task — code, docs, config, or workspace changes — to catch mistakes the agent would otherwise hand back to the user.
argument-hint: "Optional: what was changed, or the check to emphasise"
---

# self-verify

Run this before you say "done". The goal is simple: an agent should validate its own work so it can run long and independently, and only hand back results that hold up.

This is the lightweight, generic gate. For heavier or domain-specific cases use the matching skill instead or as well: `plan-verify-execute` (staged risky work), `tdd` (test-first code), `audit` (deep review), or a project's own verification skill (e.g. a project's `gymmie-verification`) / `docs/adrs`.

## Checklist

Work through what applies; skip what doesn't, but say which you skipped.

1. **Re-read the diff.** Look at every changed line. Does each one trace back to the task? Remove stray edits, debug leftovers, and unrelated changes.
2. **Run the relevant check.** Pick the cheapest check that actually exercises the change:
   - Code: lint, typecheck, the affected tests, a build, or a quick run/smoke.
   - Odin/workspace: `bash .odin/scripts/odin-doctor.sh`; if instructions changed, confirm `sync-instructions.sh` ran and the generated files match; if skills changed, confirm `sync-skills.sh` ran.
   - Docs/data: re-read for correctness, check links/paths resolve, spot-check numbers.
3. **Check against the goal.** Does the result do what was actually asked — including the edge the user implied but didn't spell out? Did you miss any part of a multi-part request?
4. **Check the guardrails.** No secrets written to memory/wiki/raw/docs. No durable memory or instruction edits without approval. No generated files hand-edited. Nothing destructive done without asking.
5. **State the verification.** In your summary, say what you verified and how, and call out anything you could **not** verify (and why) rather than letting it pass silently.

## When a check fails

Fix it and re-run the check, don't report around it. If you can't fix it (needs a decision, external access, or it's pre-existing and out of scope), say so plainly and separate it from the work you did change.

## Prefer a standing gate

If the same class of mistake keeps needing a manual check, that's a signal to propose a deterministic gate (a pre-commit hook, a test, a script) so it runs every time instead of when an agent remembers. Surface it — and let `reflect` turn it into a proposal.
