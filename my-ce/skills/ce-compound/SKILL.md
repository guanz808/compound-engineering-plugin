---
name: ce-compound
description: Extract learnings from a completed session and write them into CLAUDE.md and docs/solutions/. This is the step that makes each project faster than the last. Use after finishing a feature or review session. Triggers on "compound", "ce:compound", "save learnings", or "capture what I learned".
---

# /ce:compound

Capture what was learned this session and write it into `CLAUDE.md` and `docs/solutions/`. This is the most important step — skip it and the work doesn't compound.

---

## Phase 1 — Gather session context

Collect what happened:
- `git log --oneline -15` — what was built or fixed
- Any resolved todos from `todos/` (files with `status: done`)
- The plan that was executed, if there's one in `docs/plans/`
- Ask the user: "Anything specific you want to make sure we capture from this session?"

---

## Phase 2 — Read current state

1. Read `CLAUDE.md` — know what's already there before proposing anything
2. Read `docs/solutions/` frontmatter if the directory exists — avoid duplicates

---

## Phase 3 — Identify what's worth capturing

Think through four questions:

**1. What patterns worked?**
Something that proved to be the right approach for this codebase. Should be done this way every time. Specific enough that a future session would do something differently because of it.

**2. What anti-patterns appeared?**
Something that caused a bug, rework, or confusion. A tempting shortcut with hidden costs. A pattern that violates this project's architecture.

**3. What could be caught earlier next time?**
A P1/P2 review finding, a rework commit ("fix X" followed by "actually fix X") — what rule would prevent the first mistake?

**4. Is there a generalizable solution worth documenting?**
A non-obvious fix to a problem category that will recur. A decision with meaningful tradeoffs. Only for genuinely reusable insights — not every session warrants a solution doc.

---

## Phase 4 — Propose to the user, never auto-write

Show all proposed changes before writing anything. For each:

### CLAUDE.md additions

Show as proposed text in context:

```
Add to "Patterns that work well":

### <title>
**When:** <context — when does this apply>
**Do:** <specific, actionable pattern>
**Because:** <the reason — what does this enable or prevent>
```

```
Add to "Anti-patterns to avoid":

### <title>
**Symptom:** <what it looks like when someone does this>
**Instead:** <the better approach>
**Why:** <what goes wrong if you don't>
```

### Solution doc (only if genuinely generalizable)

Show a preview:
```
New file: docs/solutions/<category>/<YYYY-MM-DD-slug>.md

---
title: <title>
date: YYYY-MM-DD
tags: [<language>, <feature-type>, <keywords>]
problem: <one-line summary>
---

<content preview>
```

For each proposal, ask: **keep as-is, edit, or skip?**

---

## Phase 5 — Write approved entries

**CLAUDE.md:** Append approved entries to the correct section. Never rewrite or delete existing entries.

**Solution doc:** Create the file in `docs/solutions/<category>/`. Create the directory if it doesn't exist. Use categories: `auth`, `data`, `frontend`, `testing`, `infra`, `other`.

Solution doc format:
```markdown
---
title: <title>
date: YYYY-MM-DD
tags: [<language>, <feature-type>, <keywords>]
problem: <one-line summary of the problem this solves>
---

## Problem
What the problem was and why it was non-obvious.

## Solution
What worked. Specific enough to apply directly.

## Why this approach
Tradeoffs considered. Why this over alternatives.

## Gotchas
What to watch out for when applying this.
```

---

## Phase 6 — Confirm

```
Compounded:
- Added N entries to CLAUDE.md
- Created N solution docs in docs/solutions/

Next session, Claude will automatically apply these patterns when planning and reviewing.
```

---

## Rules

- **Never write without explicit user approval** — show all proposals first
- Quality over quantity: 1–2 sharp entries beat 5 vague ones
- The test for a good CLAUDE.md entry: would a future Claude session do something differently because of it?
- Solution docs are optional — most sessions produce 0; only create one for genuinely reusable insights
- Never modify or delete existing CLAUDE.md entries — only append
