# lockedin-project-structure

A reusable structure for new and existing projects: agent rules (`AGENTS.md`), one task queue (`TASKS.md`), and a `docs/` tree that keeps ideas, plans, and features apart. Built from real projects, not from a whiteboard.

> **This file describes the template.** In a new project, replace it with the project's own README (task `OWNER1` in `TASKS.md`).

## Two ways in

- **New project** — on GitHub, **Use this template → Create a new repository**, clone it, open Claude Code, and work the queue from `TASKS.md` (first task: `OWNER1`). Without GitHub: `npx degit <you>/lockedin-project-structure my-project`, then `git init`.
- **Existing project** — copy the files in without overwriting yours, merge the rules into your current `AGENTS.md`, and seed the queue from what already ships.

Both paths, in order, are in [`ADOPTION.md`](ADOPTION.md). Read it first; delete it once your queue is running.

## What you get

```
AGENTS.md              # rules: lanes, pre-flight, handoff, ideas, skills
CLAUDE.md              # imports AGENTS.md so Claude Code and opencode read the same rules
TASKS.md               # the work queue + Log
README.md              # replace with the project's own
ADOPTION.md            # how to adopt this structure — template-only; delete after
.gitignore
.claude/skills/        # project skills, one folder each (read by Claude Code and opencode)
docs/
  1-ideas/               # raw ideas, I##-slug.md   (README + _template)
  2-plans/               # build scopes            (_template)
  3-features/            # one folder per feature  (_template/ with assets/)
  9-archive/             # finished one-off docs
  0-strategy.md          # evergreen: the bet, rules, gates, glossary
  3-features.md          # catalog, one short entry per feature
  4-contract-requests.md # UI → BACKEND asks
  README.md              # how docs work + conventions
```

## How work flows

```
idea → triage → plan → tasks → feature doc
```

- **Idea** — the owner's words, captured in `docs/1-ideas/`, never built directly.
- **Triage** — owner or BACKEND sets a verdict: `build`, `park`, `drop`.
- **Plan** — `docs/2-plans/`, ends with its work split by lane.
- **Tasks** — one row per item in `TASKS.md`, each owned by `UI` (Claude Code), `BACKEND` (opencode), or `OWNER` (human).
- **Done** — `Done when` observed, row flagged `[x]`, one line in the Log.

## Customising

The lane names, the tools behind them, and the file-ownership table live in `AGENTS.md`. Change them here once and every project created from the template inherits the change.
