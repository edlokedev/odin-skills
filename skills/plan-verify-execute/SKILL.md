# plan-verify-execute

Purpose: enforce staged work.

## Artifact Locations

For single-project work, durable artifacts live under:

```txt
projects/<project>/docs/plans/
projects/<project>/docs/reviews/
projects/<project>/docs/adrs/
projects/<project>/docs/handoffs/
```

For cross-project or Odin-level work, durable artifacts live under:

```txt
initiatives/<date-slug>/
```

`.odin/runs/<run-id>/` is only the temporary session envelope and should contain links to durable artifacts.

## Flow

1. IDENTIFY SCOPE
   - One project.
   - Multiple projects.
   - Odin/system-level.

2. CHOOSE ARTIFACT LOCATION
   - One project -> `projects/<project>/docs/`.
   - Cross-project/Odin -> `initiatives/<date-slug>/`.

3. CREATE RUN ENVELOPE
   - `.odin/runs/<run-id>/task.md`.
   - `.odin/runs/<run-id>/links.md`.

4. PLAN
   - Read context.
   - Write plan to the durable location.
   - Link it from `.odin/runs/<run-id>/links.md`.
   - Do not edit source.

5. VERIFY
   - Review plan critically.
   - Write critique to the durable location.
   - Link it from `.odin/runs/<run-id>/links.md`.
   - Do not edit source.

6. FINALIZE
   - Write final plan to the durable location.

7. EXECUTE
   - Edit only files covered by final plan.
   - Write execution log to `.odin/runs/<run-id>/execution-log.md`.

8. VERIFY AGAIN
   - Check diff, tests, risks.
   - Write verification to the durable location or run envelope, depending on whether it should outlive the run.

9. MEMORY
   - Write proposed memory to `memory/candidates` if useful.
