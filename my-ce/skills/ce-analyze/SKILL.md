---
name: ce-analyze
description: Deep structural analysis of an existing codebase or specific module before making changes. Use when you're new to a project, inheriting code, or need to understand a complex area before refactoring. Produces a clear map of what's there, how it fits together, and where the risks are. Triggers on "analyze", "ce:analyze", "understand this code", "what does this do", "before I touch this", or "explain this codebase".
---

# /ce:analyze

Read and understand an existing codebase or module deeply before making any changes. Produces a structured analysis report.

---

## Phase 1 — Determine scope

Check in order:
1. A path passed directly: `/ce:analyze src/payments/`
2. Ask the user: "What area do you want me to analyze? (whole project, a specific module, or a particular file?)"

If the whole project: start with the root directory structure, then drill into the most important areas. Don't try to read every file — find the spine of the codebase first.

---

## Phase 2 — Orient

Before reading any code, get the lay of the land:

1. Read `CLAUDE.md` if it exists — absorb any existing context
2. Read `README.md` or any top-level docs
3. List the directory structure 2–3 levels deep
4. Check `package.json`, `Gemfile`, `requirements.txt`, `go.mod`, `Cargo.toml`, or equivalent — understand the stack, dependencies, and entry points
5. Scan `git log --oneline -20` — recent activity shows what's been touched and what areas are hot

---

## Phase 3 — Read the code

Work systematically through the target area:

**For a whole project:**
- Find the entry point (main file, index, app.js, application.rb, etc.) and read it
- Follow the most important data flows from entry to persistence
- Read the most-changed files from git log
- Sample test files to understand what's considered important enough to test

**For a specific module or directory:**
- Read every file in the target area
- Map out what each file/class/function is responsible for
- Trace the inputs and outputs
- Find all external dependencies (what this module calls, what calls it)

**For a specific file:**
- Read the whole file
- Understand every function/method: what it does, what it takes, what it returns
- Note any side effects, global state, or hidden dependencies

---

## Phase 4 — Write the analysis report

Save to `docs/analysis/YYYY-MM-DD-<slug>.md`.

```markdown
# Analysis: <what was analyzed>

**Date:** YYYY-MM-DD
**Scope:** <what was read>
**Stack:** <languages and key frameworks detected>

## What this does
One paragraph. Plain language. What problem does this code solve?

## Architecture overview
How is it structured? What are the main components and how do they relate?
Keep this to 3–5 bullet points — a map, not a textbook.

## Data flow
How does data enter, transform, and exit? Trace the main path end-to-end.

## Key files
| File | Responsibility |
|---|---|
| path/to/file.ext | What it does in one sentence |

## Dependencies and coupling
- What external services or libraries does this depend on?
- Which internal modules are tightly coupled?
- What would break if you changed X?

## Test coverage
- What's tested? What's not?
- Where are the gaps that matter most?

## Risks and landmines
Things to know before touching this code. Gotchas, fragile areas,
implicit assumptions, things that look safe but aren't.

## Suggested entry points for changes
If you were going to modify this area, where would you start and why?

## Open questions
Things that are unclear from reading the code alone — worth asking before changing anything.
```

---

## Phase 5 — Hand off

Show the saved path. Suggest a next step:
- "Ready to find improvement opportunities? Try `/ce:ideate`"
- "Know what you want to change? Try `/ce:brainstorm`"
- "Got a bug to chase? Try `/ce:debug`"

---

## Rules

- Read before you opine — don't make claims about how the code works without reading it
- Be honest about uncertainty — "this appears to" is better than confident claims about code you haven't fully traced
- Risks section is the most important part — don't soften it
- No external lookups — analyze what's actually there
- Never make changes during analysis — this is read-only
