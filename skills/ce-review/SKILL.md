---
name: ce-review
description: Review recent code changes thoroughly. Covers security, performance, architecture, and language quality. Saves findings to todos/ for triage. Use before committing, before opening a PR, or any time you want a careful review. Triggers on "review", "ce:review", "review my changes", or "check my code".
---

# /ce:review

Review code changes and save prioritized findings to `todos/` for triage.

---

## Phase 1 — Find what to review

Check in this order:
1. A path passed directly: `/ce:review src/auth/`
2. Staged changes: run `git diff --cached`
3. Unstaged changes: run `git diff HEAD`
4. Ask the user if nothing found

---

## Phase 2 — Read context

Before reviewing:
1. Read `CLAUDE.md` — extract review standards, anti-patterns, and project conventions. These take precedence.
2. Find a related plan in `docs/plans/` if one exists (match by date or slug) — use it to check plan adherence
3. Read `docs/solutions/` frontmatter for any relevant past decisions

---

## Phase 3 — Review the code

Work through the diff or files systematically. Cover all four areas:

### Security
- Injection risks: SQL, command, template, path traversal
- Auth/authz: missing checks, checks that can be bypassed, hardcoded credentials
- Input validation: user input used without sanitization, trusting client-supplied values
- Data exposure: secrets in logs, sensitive data in API responses unnecessarily

### Performance
- N+1 queries: a query inside a loop that could be batched
- Missing indexes on columns used in WHERE/JOIN/ORDER BY
- Expensive operations in loops that could be hoisted
- Unnecessary re-computation of stable values
- Large fetches where a subset would do

### Architecture
- Does the code follow CLAUDE.md patterns? Flag any violations by name
- Does it match the plan's intent, or did scope creep in?
- Logic in the right layer (business logic not in controllers, etc.)
- New coupling between components that should be independent
- Unnecessary abstraction or premature future-proofing (YAGNI)

### Language quality
Adapt to whatever language(s) you detect:

**Python:** type hints on public functions, specific exception handling (no bare `except:`), mutable defaults, context managers for files/connections

**TypeScript/JS:** unexplained `any` types, unhandled promises, missing awaits, `console.log` left in, `const` vs `let`

**Ruby:** guard clauses over deep nesting, specific rescue, method length, frozen string literals

**Other languages:** apply equivalent idiomatic checks; if you don't know the language well enough, say so and stick to universal checks

**Universal (all):** dead code, magic numbers/strings that should be constants, missing or weak tests, debug output left in

---

## Phase 4 — Severity calibration

**P1 — Critical:** Exploitable in production, causes data loss or crashes, or auth bypass. Would block a PR.
**P2 — Important:** Degrades maintainability significantly, will cause issues at scale, violates established project conventions.
**P3 — Minor:** Style, small improvements, optional fixes.

Most reviews have 0–1 P1s. Don't inflate severity. If you're finding many P1s, recalibrate.

---

## Phase 5 — Save findings to `todos/`

Create one file per finding. Number sequentially (check existing files first).

Filename: `todos/<NNN>-ready-<priority>-<slug>.md`

```markdown
---
status: ready
priority: p1
reviewer: security
---

# <short title>

**File:** path/to/file.ext (line N if known)
**Issue:** Clear description of the problem.
**Why it matters:** Impact if left unfixed.
**Fix:** Specific, actionable recommendation.
```

Create `todos/` if it doesn't exist.

---

## Phase 6 — Print summary

```
Review complete.

P1 — Critical:
  [ ] <title> → todos/001-ready-p1-<slug>.md

P2 — Important:
  [ ] <title> → todos/002-ready-p2-<slug>.md

P3 — Minor:
  [ ] <title> → todos/003-ready-p3-<slug>.md

What's working well:
  - <genuine specific callout>
  - <genuine specific callout>

Run /ce:triage to work through findings.
```

Always include at least 1–2 genuine "working well" callouts. Review is not only criticism.
If no issues at a severity level, omit that section.

---

## Rules

- Be specific — "this could be a problem" is not a finding. "Line 47: user input is concatenated into an SQL query string" is.
- Reference CLAUDE.md patterns by name when citing violations
- Check plan adherence if a plan exists — flag scope creep and gaps
- No external lookups — review against the code and context you're given
