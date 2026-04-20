# Project

> Copy this file into your project root and fill it in. Claude reads it at the start of every session.
> The more specific it is, the better every plan, review, and compound gets.
> After each /ce:compound session, new entries are appended here automatically (with your approval).

---

## What this project is

compound-engineering-plugin — a Claude Code plugin that implements a compound engineering workflow (ideate → analyze → brainstorm → plan → work → debug → review → triage → compound). Skills are plain markdown files in `skills/`. No external services.

## Stack

- Claude Code plugin (`.claude-plugin/plugin.json`)
- Skills: plain markdown (`skills/ce-*/SKILL.md`)
- No build system, no dependencies, no runtime

## Conventions

- Skill files use YAML frontmatter (`name`, `description`) followed by markdown phases
- Each skill follows a numbered Phase structure with a Rules section at the end
- Filenames for generated docs: `YYYY-MM-DD-<slug>.md` (kebab-case, 3–5 words)
- All user-facing output is written to `docs/` or `todos/` — never modify project files silently
- Always show proposals before writing; never auto-write without explicit user approval

## Patterns that work well

<!-- Added by /ce:compound -->

## Anti-patterns to avoid

<!-- Added by /ce:compound -->

## Architecture decisions

| Decision | Chose | Over | Because | Date |
|---|---|---|---|---|
| Plugin structure | Single root install | Nested `my-ce/` subfolder | Eliminates duplicate files; `claude /plugin install --dir .` works from root | 2026-04-19 |
| Skill format | Plain markdown phases | Code/scripts | Skills are instructions to Claude, not executable code — markdown keeps them editable and transparent | — |

## Review standards

- P1: Skill produces incorrect output, writes files without approval, or breaks the compound loop
- P2: Inconsistent phase structure, missing Rules section, unclear acceptance criteria in tasks
- P3: Wording improvements, minor formatting

## Open questions

<!-- Things needing a decision — remove when resolved -->
