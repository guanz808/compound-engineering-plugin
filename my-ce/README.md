# my-ce — Personal Compound Engineering Plugin

A lean Claude Code plugin for planning, reviewing, and building up personal engineering knowledge over time.
No external services. No MCP servers. No multi-agent orchestration. Just Claude.

---

## Install

```bash
claude /plugin install --dir ./my-ce-plugin
```

Then copy `CLAUDE.md` from this folder into your project root and fill in the top section.

---

## The loop

```
brainstorm → plan → (write code) → review → triage → compound → repeat
```

| Command | What it does |
|---|---|
| `/ce:ideate` | Scan the codebase for high-impact improvements and rank them |
| `/ce:analyze` | Deep structural read of existing code — map architecture, data flow, risks, and entry points before touching anything |
| `/ce:brainstorm` | Clarify a fuzzy idea through codebase research + Q&A → `docs/brainstorms/` |
| `/ce:plan` | Produce an atomic task list from a brainstorm or idea → `docs/plans/` |
| `/ce:work` | Execute the plan task by task; auto-detects build commands and runs a fix loop on failures |
| `/ce:debug` | Diagnose and fix build failures, test failures, or runtime errors with an iterative fix loop |
| `/ce:review` | Review staged/recent changes across security, performance, architecture, and language quality → `todos/` |
| `/ce:triage` | Step through `todos/` one at a time — fix, skip, or defer each finding |
| `/ce:compound` | Extract learnings → propose entries for `CLAUDE.md` and `docs/solutions/` (you approve each) |

---

## What gets created in your project

```
CLAUDE.md                        ← read every session; your patterns accumulate here
docs/
├── brainstorms/                 ← one file per brainstorm session
├── plans/                       ← one file per plan
└── solutions/                   ← searchable solved-problem docs (YAML frontmatter)
    ├── auth/
    ├── data/
    ├── testing/
    └── other/
todos/
├── 001-ready-p1-<slug>.md       ← review findings, worked through with /ce:triage
└── 002-ready-p2-<slug>.md
```

**Commit all of these.** They are the compounding asset — each session builds on the last.

---

## How compounding works

Every `/ce:compound` session proposes new entries for `CLAUDE.md`. You approve what goes in.

From that point forward, every brainstorm reads `CLAUDE.md` before asking questions. Every plan reads it before writing tasks. Every review checks your conventions against it.

After 5–10 sessions your `CLAUDE.md` has your actual patterns, your actual anti-patterns, your actual architecture decisions. The plugin gets meaningfully better at your codebase over time.

---

## No external services

Zero network calls. No Context7, no Proof, no telemetry, no MCP servers. Everything runs through Claude's built-in reasoning working on your local files.

---

## Customize anything

All files are plain markdown — edit them to change any behavior:

```
skills/
├── ce-ideate/SKILL.md        ← surface high-impact improvements
├── ce-analyze/SKILL.md       ← deep read of existing code before touching it
├── ce-brainstorm/SKILL.md    ← clarify ideas through research + Q&A
├── ce-plan/SKILL.md          ← produce atomic task lists
├── ce-work/SKILL.md          ← execute plans with build validation + fix loop
├── ce-debug/SKILL.md         ← diagnose and fix build/test/runtime failures
├── ce-review/SKILL.md        ← security, performance, architecture, language review
├── ce-triage/SKILL.md        ← step through findings one at a time
└── ce-compound/SKILL.md      ← extract learnings into CLAUDE.md
CLAUDE.md                     ← your project context (copy into every project)
```

Re-run the install command after any edits to reload.
