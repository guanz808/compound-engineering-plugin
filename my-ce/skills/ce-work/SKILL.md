---
name: ce-work
description: Execute a plan step by step with progress tracking. Use after /ce:plan to implement the tasks. Tracks progress in the plan file itself by checking off completed tasks. Triggers on "work", "ce:work", "start working", "implement the plan", or "execute".
---

# /ce:work

Implement the current plan task by task, tracking progress as you go.

---

## Phase 1 — Find the plan

Check in order:
1. A path passed directly: `/ce:work docs/plans/2024-01-15-auth.md`
2. The most recent file in `docs/plans/` with `status: active`
3. Ask the user which plan to work from

Read the plan fully. Understand the approach, all tasks, and acceptance criteria before starting.

---

## Phase 2 — Read context

1. Read `CLAUDE.md` — apply all conventions and patterns while implementing
2. Read any `docs/solutions/` entries referenced in the plan
3. Read the files the plan says will be affected — understand what's there before changing it

---

## Phase 3 — Confirm before starting

Show the user:
```
Working from: docs/plans/<filename>.md

Next task: <first unchecked task>
Acceptance: <its acceptance criterion>

Starting now — let me know if you want to adjust anything first.
```

If there are already checked tasks in the plan, show progress: "3 of 7 tasks done. Resuming from task 4."

---

## Phase 4 — Execute tasks in order

For each unchecked task:

1. **Read** the relevant files before making changes
2. **Implement** the task following CLAUDE.md conventions exactly
3. **Verify** the acceptance criterion is met — run tests if applicable (`npm test`, `pytest`, `rspec`, etc. — detect from the project)
4. **Check off** the task in the plan file: change `- [ ]` to `- [x]`
5. **Report** briefly: "✓ Task 2 done — added null check, all tests pass"

Then move to the next task. Don't ask for confirmation between tasks unless something unexpected comes up.

---

## Phase 5 — Build validation after each task

After implementing each task, detect and run the appropriate command:

**Auto-detection order:**
- `package.json` with `"build"` script → `npm run build`
- `package.json` with `"test"` script → `npm test`
- `pytest.ini` / `pyproject.toml` / `setup.py` present → `pytest`
- `Gemfile` present → `bundle exec rspec`
- `Cargo.toml` present → `cargo build && cargo test`
- `go.mod` present → `go build ./... && go test ./...`
- `Makefile` with `test` target → `make test`

If the build/tests **pass**: check off the task, report briefly, move to next.

If the build/tests **fail**: enter the fix loop —
1. Read the full error output (stack trace bottom-up — root cause is usually near the bottom)
2. Find the first line in *your* code, not a library
3. Apply a targeted fix — only change what the error points to
4. Rebuild/retest
5. Repeat up to 3 times

If still failing after 3 attempts: stop, report exactly what was tried and what each attempt produced, ask the user how to proceed. Don't keep guessing.

**If the build was already failing before you started:** note it upfront and don't count it against your fix attempts — use `/ce:debug` for pre-existing failures instead.

---

## Phase 6 — Handle plan problems

If a task can't be completed as written (a design problem, not a build error):

- Describe the problem clearly
- Propose a concrete alternative
- Ask the user before proceeding differently
- Update the plan if the approach changes

Don't silently skip tasks or work around problems without flagging them.

---

## Phase 7 — Finish

When all tasks are checked off and the build is passing, update the plan status from `active` to `complete`:

```
Plan complete. All N tasks done. Build passing.

Suggested next steps:
  /ce:review   ← review what was built before committing
  /ce:compound ← capture learnings from this session
```

Never mark complete if the build is failing or acceptance criteria aren't fully met.

---

## Rules

- Follow CLAUDE.md conventions on every change — no exceptions
- Read files before editing them — never overwrite without understanding what's there
- Check off tasks as you go — the plan file is the source of truth for progress
- Always run build/tests after each task — never skip validation
- Fix build errors before moving to the next task — don't accumulate failures
- No external lookups — work from the codebase and plan only
- If the plan is ambiguous, ask before guessing
