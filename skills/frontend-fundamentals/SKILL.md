---
name: frontend-fundamentals
description: "Distilled Toss Frontend Fundamentals code-quality principles (readability, predictability, cohesion, coupling). Use for ALL frontend code work — writing or reviewing components, hooks, state management, forms, or UI logic — instead of fetching github.com/toss/frontend-fundamentals."
---

# Frontend Fundamentals (Toss) — Code Quality Reference

Good frontend code = **easily modifiable** code. Judge every change against four criteria: Readability, Predictability, Cohesion, Coupling. They conflict at times — pick per situation based on what is most likely to change.

## 1. Readability

Code is easy to read when the reader holds few contexts at once and reads top-to-bottom without jumping around.

**Reduce context:**
- Separate code that doesn't run together: mutually exclusive branches (e.g. admin vs viewer UI) go in separate components/functions, not interleaved conditionals — each unit then handles one state.
- Abstract implementation details: wrap cross-cutting logic (auth guard, dialog plumbing) in dedicated components/HOCs so each reading level shows only essentials — people track only ~6 contexts at once.
- Don't split code by logic *type* (all queries here, all state there); a `usePageState()`-style catch-all hook grows unbounded and causes broad re-renders. Split by purpose instead.

**Name things:**
- Name complex conditions as variables (`isSameCategory`, `isPriceInRange`) instead of nesting anonymous predicates. Skip naming when logic is trivial and single-use (`x => x * 2`) — naming there adds noise.
- Name magic numbers as constants (`ANIMATION_DELAY_MS = 300`, `HTTP_NOT_FOUND = 404`); a bare literal hides intent.

**Read top-to-bottom:**
- Reduce eye movement: inline simple policy/lookup logic (switch or inline object) at the point of use rather than forcing jumps across files/functions.
- Simplify nested ternaries: rewrite as sequential `if` statements (IIFE if an expression is needed) so condition→value mapping is scannable.
- Write comparisons left-to-right like math: `80 <= score && score <= 100`, not `score >= 80 && score <= 100`-style backtracking.

## 2. Predictability

Behavior must be inferable from name, parameters, and return type alone.

- Don't reuse a well-known name for different behavior: a wrapper around an HTTP lib that adds auth is `httpService.getWithAuth()`, not `http.get()`. Same name ⇒ same behavior.
- Unify return types of same-kind functions: all API hooks return the same shape (e.g. react-query `Query`); all validators return one discriminated union (`{ ok: true } | { ok: false; reason: string }`) so the compiler enforces correct handling.
- Reveal hidden logic: keep only what the signature promises inside the function; move side effects (logging, analytics) to the caller. A hidden side effect failing breaks "unrelated" features.

## 3. Cohesion

Code that must change together stays together, so edits can't silently miss a piece.

- Colocate files modified together in one feature/domain directory (component + hooks + utils per feature), not by file type. Dependencies become visible; deleting a feature is deleting a folder; cross-domain imports become obvious red flags.
- Extract shared magic numbers into one constant when multiple places must stay in sync (animation delay used by both the animation and its test) — changing one literal and not the other breaks silently.
- Form cohesion — choose the unit deliberately:
  - Field-level (per-input validation/state) when fields are independent, reusable, or need async/complex validation.
  - Form-level (one schema/state for all fields) when fields form one business function or reference each other (password confirm, cross-field totals).

## 4. Coupling

Minimize the blast radius of any change.

- Manage responsibilities one by one: no god-hooks/components handling "all X" (all query params, all page state). One hook per responsibility (`useCardIdQueryParam()`) keeps modification impact narrow.
- Allow duplication over premature abstraction: if consumers of shared logic may diverge (different UI, logging, behavior later), duplicated code is safer than a shared hook that every change must regression-test across all users. Centralize only what is genuinely and durably identical — confirm with the team.
- Eliminate props drilling via composition: pass children/components from the parent that owns the data instead of threading props through intermediates; every drilled prop couples the whole chain to its shape.

## Key tensions (decide, don't dogmatize)

- Readability vs cohesion/abstraction: extracting variables, constants, and wrappers for cohesion can reduce at-a-glance readability; inlining for readability can duplicate values that must stay in sync.
- Coupling vs cohesion: deduplicating raises cohesion but couples all call sites; allowing duplication lowers coupling but risks unsynchronized edits.
- Wrong abstraction is worse than duplication — abstract only when behavior is truly shared and stable.
- Prioritize by risk: optimize the criterion whose violation makes the *likely next change* dangerous.

Source: https://github.com/toss/frontend-fundamentals (docs: https://frontend-fundamentals.com/code-quality/en/), fetched 2026-07-04.
