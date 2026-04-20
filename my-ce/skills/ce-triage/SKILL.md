---
name: ce-triage
description: Step through review findings in todos/ one by one and decide what to do with each. Use after /ce:review to filter and action findings. Triggers on "triage", "ce:triage", or "work through findings".
---

# /ce:triage

Work through `todos/` findings one at a time — fix, skip, or defer each one.

---

## Phase 1 — Load findings

Read all files in `todos/` with `status: ready`. Sort: P1 first, then P2, then P3. Within each priority, sort by filename (sequential order).

If `todos/` is empty or has no `ready` items, tell the user and stop.

---

## Phase 2 — Present one at a time

Show each finding like this:

```
Finding 2 of 7 — P1 (security)
────────────────────────────────
SQL injection in search query
File: app/search.py, line 47

Issue: User input is concatenated directly into the SQL query string.
Why it matters: An attacker can read or modify any data in the database.
Fix: Use parameterized queries — cursor.execute("SELECT * FROM items WHERE name = ?", [query])

[F] Fix now   [S] Skip (won't fix)   [L] Later   [E] Edit
```

Wait for the user's choice before showing the next finding.

---

## Phase 3 — Handle each choice

**F — Fix now:**
Make the fix immediately. When done, update the todo file: change `status: ready` to `status: done`. Move to next finding.

**S — Skip:**
Ask for a one-line reason (optional). Delete the todo file. Move to next finding.

**L — Later:**
Leave the file unchanged. Move to next finding.

**E — Edit:**
Let the user change the priority or add notes. Save the file. Move to next finding.

---

## Phase 4 — Summary

After all findings have been presented:

```
Triage complete.
Fixed: N  |  Skipped: N  |  Later: N

Remaining in todos/: N items
Run /ce:compound to capture learnings from this session.
```

---

## Rules

- One finding at a time — never dump the full list
- P1s always come first
- Never auto-fix without explicit user choice
- Keep `todos/` clean — skipped items are deleted, not accumulated
