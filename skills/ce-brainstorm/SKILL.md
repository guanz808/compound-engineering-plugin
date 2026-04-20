---
name: ce-brainstorm
description: Explore and clarify a feature idea or problem before planning. Use when requirements are fuzzy or you're not sure what to build. Run this before /ce:plan. Triggers on "brainstorm", "ce:brainstorm", or when the user has an idea they want to think through before coding.
---

# /ce:brainstorm

Turn a vague idea into a clear requirements doc through codebase research and focused Q&A.

---

## Phase 1 — Read context first

Before asking the user anything:

1. Read `CLAUDE.md` — absorb project conventions, patterns, and any open questions
2. Search `docs/solutions/` if it exists — scan frontmatter for past decisions on similar problems
3. Grep the codebase for terms related to the idea — find similar existing functionality, affected files
4. Check `docs/brainstorms/` for any related past brainstorms

This takes 1–2 minutes. Do it silently. Use it to ask sharper questions.

---

## Phase 2 — Focused Q&A (3 questions max)

Ask one question at a time. Only ask what's genuinely unclear after the research. Stop as soon as you have enough to write the doc.

Good questions:
- What does "done" look like? How will you know it works?
- What's explicitly out of scope for this?
- Any constraints — backward compatibility, specific libraries, timeline?

Skip any question you can already answer from the codebase research.

---

## Phase 3 — Write the requirements doc

Save to `docs/brainstorms/YYYY-MM-DD-<slug>.md` where slug is 3–5 words, kebab-case.

```markdown
# <title>

**Date:** YYYY-MM-DD
**Status:** draft

## Problem
What hurts and why it matters. One paragraph.

## Goal
What we're building. Concrete and testable — "user can do X" not "improve Y".

## Relevant codebase context
Files likely affected. Existing patterns to follow. Past solutions referenced.

## Constraints
Tech, backward compatibility, timeline, libraries to avoid.

## Out of scope
Explicit list. Prevents scope creep during planning.

## Open questions
Still unclear — to be resolved during /ce:plan.
```

---

## Phase 4 — Hand off

Tell the user where the doc was saved. Ask: "Ready to turn this into a plan with `/ce:plan`?"

---

## Rules

- Always research the codebase before asking any questions
- Never skip the doc — even simple ideas benefit from writing them down
- Ask questions one at a time, conversationally
- No external lookups — use only the codebase and Claude's built-in knowledge
- Short-circuit if the idea is already well-defined: skip Q&A and go straight to writing the doc
