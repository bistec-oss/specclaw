# specclaw plugin — Claude Code instructions

## Project Context (`context.md`)

Every specclaw project can have a `.specclaw/context.md` — a living architecture document that captures project-level coding rules, patterns, style guides, and decisions. It is committed to the project repo (not gitignored) so it is shared across the team and reviewable in PRs.

**What it contains:** Architecture overview, coding style and conventions, key patterns, technology decisions, constraints (what not to do), and a log of recent decisions.

**How to create/edit it:** Use `/specclaw:context` — sub-commands: `show`, `add`, `edit`, `reset`.

**How it is used automatically:**
- `/specclaw:plan` reads `context.md` before generating spec, design, and tasks — decisions and constraints are applied throughout.
- `/specclaw:build` injects `context.md` into every coding agent's context payload via `specclaw-build-context`.
- `/specclaw:verify` checks the implementation against `context.md` rules in addition to spec acceptance criteria.
- `/specclaw:pr` and `/specclaw:pr-azdo` rewrite `context.md` after each merged change via `specclaw-update-context` — new decisions, patterns, and constraints from the change are merged in; stale information is replaced.

**Architecture-doc model:** `context.md` is always current. It is not an append log — it is rewritten to reflect the project's present state. Git history is the audit trail.

## Lifecycle

`propose` → `plan` → `build` → `verify` → `pr` → _(context auto-updated)_

Each phase has a corresponding skill. Run them in order. See individual SKILL.md files under `skills/` for details.

## Scripts

All executable scripts live in `bin/`. Key ones:

| Script | Purpose |
|--------|---------|
| `specclaw-ensure-init` | Idempotently init `.specclaw/` |
| `specclaw-build-context` | Build coding agent payload (includes context.md) |
| `specclaw-update-context` | Output LLM prompt to rewrite context.md post-merge |
| `specclaw-update-status` | Regenerate `.specclaw/STATUS.md` dashboard |
| `specclaw-gh-sync` | GitHub Issues sync |
| `specclaw-pr` | Create GitHub PR (enforces test policy, triggers context update) |
| `specclaw-validate-change` | Check phase prerequisites |

## Templates

Templates live in `templates/`. `context.md` is the seed for new projects — copy it to `.specclaw/context.md` or let `/specclaw:context add` create it automatically.
