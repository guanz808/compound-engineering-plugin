---
name: ce-ideate
description: Discover high-impact project improvements through divergent ideation and adversarial filtering. Use when you're not sure what to build next, want to find the most valuable improvements in the codebase, or need fresh ideas. Triggers on "ideate", "ce:ideate", "what should I work on", or "what would improve this project".
---

# /ce:ideate

Proactively surface the highest-impact improvements in your codebase. Use this when you don't have a specific feature in mind and want to find what's most worth building next.

---

## Phase 1 — Read context

1. Read `CLAUDE.md` — understand the project, its conventions, any open questions
2. Read `docs/solutions/` frontmatter — understand what's been solved before
3. Scan recent git log: `git log --oneline -20` — understand recent activity and what areas are being touched
4. Ask the user (optional): "Any areas you've been frustrated with, or goals you're trying to hit?" — skip if they've already said

---

## Phase 2 — Divergent scan

Explore the codebase broadly to surface candidate improvements. Look across:

**Code quality signals**
- Files changed most frequently in git log (high churn = pain points)
- Long functions or files that are hard to follow
- Duplicated logic that should be extracted
- Missing or weak test coverage in important areas
- Error handling that's inconsistent or missing

**User-facing value**
- Features that are partially implemented or have obvious next steps
- Common patterns in the codebase that suggest an unmet need
- Anything that looks brittle or likely to break under load

**Developer experience**
- Slow or painful parts of the workflow (build, test, deploy)
- Config or setup that's harder than it needs to be
- Documentation gaps for non-obvious parts of the system

Generate 6–10 candidate ideas. Don't filter yet — diverge first.

---

## Phase 3 — Adversarial filtering

For each candidate, apply two filters:

**Value filter:** Would this meaningfully improve the codebase, user experience, or developer velocity? Or is it just interesting?

**Effort filter:** Is this achievable in a focused session or two, or does it require a major rearchitecture that's out of scope?

Eliminate candidates that are low value, too speculative, or too large. Keep the 3–5 strongest.

---

## Phase 4 — Present ranked ideas

Show the top ideas clearly:

```
Top improvements I found:

1. [High impact] <title>
   Why: <what's wrong now and what gets better>
   Scope: <rough sense of effort>

2. [Medium impact] <title>
   Why: <what's wrong now and what gets better>
   Scope: <rough sense of effort>

3. ...
```

Then ask: "Which of these interests you? I can kick off a `/ce:brainstorm` on whichever one you pick."

---

## Rules

- Be honest about impact — don't oversell ideas to seem useful
- Prioritize problems you actually found evidence of in the code, not hypothetical improvements
- No external lookups — work from the codebase and CLAUDE.md only
- Keep it actionable: each idea should be something that could realistically start today
