---
name: ce-plan
description: Turn a brainstorm doc or feature idea into a concrete, step-by-step implementation plan. Use after /ce:brainstorm or any time you have a clear idea and want an actionable task list before writing code. Triggers on "plan", "ce:plan", or "create a plan for".
---

# /ce:plan

Research the codebase and produce a concrete implementation plan saved to `docs/plans/`.

---

## Phase 1 — Find the input

Check in this order:
1. A path passed directly: `/ce:plan docs/brainstorms/2024-01-15-auth.md`
2. The most recent file in `docs/brainstorms/` (by date in filename)
3. Ask the user to describe the feature if neither exists

---

## Phase 2 — Research

Read everything relevant before planning:

1. **`CLAUDE.md`** — conventions, patterns, anti-patterns, architecture decisions
2. **The brainstorm doc** (if it exists) — problem, goal, constraints, open questions
3. **`docs/solutions/`** — scan frontmatter for past solutions to similar problems; read any that match
4. **Codebase** — find files related to the feature area; read key ones to understand existing patterns

Do not read the entire codebase. Be targeted. If you need more context, ask the user to point you to specific files.

---

## Phase 3 — Resolve open questions

If the brainstorm doc has open questions, try to answer them from the research. List any that are still unclear — and ask the user before continuing if they're blockers.

---

## Phase 4 — Write the plan

Save to `docs/plans/YYYY-MM-DD-<slug>.md` (same slug as the brainstorm doc if one exists).

```markdown
# Plan: <title>

**Date:** YYYY-MM-DD
**Brainstorm:** docs/brainstorms/<file>.md
**Status:** active

## Approach
2–3 sentences. What's the technical strategy and why this over alternatives?

## Tasks
- [ ] <Verb> <what> — Acceptance: <how to verify it's done>
- [ ] <Verb> <what> — Acceptance: <how to verify it's done>
- [ ] ...

## Risks / unknowns
Things that could go wrong or need early validation.

## Patterns applied
Which CLAUDE.md patterns or past solutions shaped this plan.

## Not doing
Explicit exclusions to prevent scope creep.
```

### Task quality rules

Good tasks:
- Completable in 30–90 minutes of focused work
- Start with a verb: Add, Write, Refactor, Extract, Fix, Remove, Test
- Have a concrete acceptance criterion — "works correctly" is not enough

Bad tasks:
- "Implement the feature" (too big — break it down)
- "Make it work" (not testable)
- "Clean up" (not specific)

**Max 8 tasks.** If more are needed, note "Phase 2: [description]" at the bottom and plan Phase 1 only.

---

## Phase 5 — Confirm

Tell the user the saved path. Ask: "Does this look right, or do you want to adjust anything before starting?"

---

## Rules

- Every task needs an acceptance criterion
- If the right technical approach is unclear, say so in Risks rather than guessing
- Follow existing patterns from the codebase — don't invent new ones without a reason
- No external lookups — reason from the codebase, CLAUDE.md, and past solutions only
