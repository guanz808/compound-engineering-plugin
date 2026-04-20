---
name: ce-debug
description: Diagnose and fix build failures, test failures, and runtime errors. Runs an iterative read-diagnose-fix-rebuild loop until the error is resolved or a clear path forward is identified. Use when a build is broken, tests are failing, or there's a runtime error you can't track down. Triggers on "debug", "ce:debug", "build is broken", "tests are failing", "fix this error", "why is this failing", or when you paste an error message or stack trace.
---

# /ce:debug

Diagnose a build failure, test failure, or runtime error and fix it. Runs iteratively until resolved.

---

## Phase 1 — Collect the error

Check in order:
1. Error output passed directly with the command
2. Run the build/test command and capture output: detect from project (`npm run build`, `npm test`, `pytest`, `rspec`, `cargo build`, `go test ./...`, `bundle exec rspec`, etc.)
3. Ask the user to paste the error if neither is available

Read the full error output carefully. Don't skim.

---

## Phase 2 — Read context

Before diagnosing:
1. Read `CLAUDE.md` — check for any known issues or relevant patterns
2. Check `docs/solutions/` for past fixes to similar errors (match on error type, language, or area)
3. Read the files mentioned in the stack trace or error message

---

## Phase 3 — Diagnose

Work through the error systematically:

**Identify the error type:**
- Compilation/syntax error — code that can't be parsed or type-checked
- Import/dependency error — missing module, version mismatch, circular dependency
- Runtime error — logic that fails when executed (null pointer, key error, assertion failure)
- Test failure — code that runs but produces wrong results
- Configuration error — wrong env var, missing file, bad config value
- Environment error — wrong version, missing tool, OS-specific issue

**Find the root cause:**
- Read the stack trace from bottom to top — the root cause is usually near the bottom, not the top
- Find the first line in *your* code (not a library), not just the last line before the error
- Check if the error is in the file/line it claims — sometimes the real issue is elsewhere
- Look for recent changes: `git diff HEAD~3..HEAD -- <affected files>` — did something break this?

**State the diagnosis clearly before fixing:**
```
Diagnosis: <one sentence — what is broken and why>
Root cause: <the underlying reason, not just the symptom>
Fix: <what needs to change>
```

Ask the user to confirm if the diagnosis is non-obvious or if multiple causes are plausible.

---

## Phase 4 — Fix and rebuild loop

Apply the fix, then immediately rebuild/retest. Repeat up to 5 times.

**Each iteration:**
1. Make the targeted fix — change only what's needed to address the diagnosed cause
2. Run the build/test command again
3. Read the full output

**If it passes:** go to Phase 5.

**If it fails with the same error:** the fix didn't address the root cause — re-diagnose more carefully.

**If it fails with a different error:** the first error is resolved; treat the new error as a fresh diagnosis starting from Phase 3.

**If still failing after 5 iterations:** stop. Report what was tried, what each attempt produced, and what you believe the remaining issue is. Don't keep guessing.

---

## Phase 5 — Verify and report

Once the build/tests pass:

```
Fixed: <one sentence describing what was wrong and what was changed>

Files changed:
- path/to/file.ext — what was changed and why

Build output: PASSING

Suggested next steps:
  /ce:review    ← review the fix before committing
  /ce:compound  ← capture this as a pattern to avoid in future
```

---

## Phase 6 — Offer to compound

If the bug reveals a pattern worth remembering (a common mistake, a gotcha in this codebase, a class of error to avoid), ask:
"This looks like something worth capturing — want to run `/ce:compound` to add it to `CLAUDE.md`?"

---

## Rules

- Read the error before touching code — don't guess based on the error type alone
- Fix the root cause, not the symptom — suppressing an error message is not a fix
- Change only what's needed — don't refactor unrelated code while debugging
- Be honest if you're stuck — 5 failed iterations means escalate, not keep guessing
- No external lookups — debug against the actual code and error output
- Never mark the bug as fixed until the build/tests actually pass
