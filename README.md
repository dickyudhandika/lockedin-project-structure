# lockedin-project-structure

A reusable structure for new and existing projects: who runs what (`SETUP.md`), agent rules (`AGENTS.md`), one task queue (`TASKS.md`), and a `docs/` tree that keeps ideas, plans, and features apart. Built from real projects, not from a whiteboard.

> **This file describes the template.** In a new project, replace it with the project's own README (task `OWNER1` in `TASKS.md`).

## Two ways in

- **New project** — on GitHub, **Use this template → Create a new repository**, clone it, open your coding agent, and let it run the setup interview (Rule 0) before working the queue from `TASKS.md` (first task: `OWNER1`). Without GitHub: `npx degit <you>/lockedin-project-structure my-project`, then `git init`.
- **Existing project** — copy the files in without overwriting yours, merge the rules into your current `AGENTS.md`, and seed the queue from what already ships.

Both paths, in order, are in [`ADOPTION.md`](ADOPTION.md). Read it first; delete it once your queue is running.

## What you get

```
SETUP.md               # which agent + model run each role — filled per person, first
AGENTS.md              # rules: setup, lanes, pre-flight, handoff, ideas, skills, naming
CLAUDE.md              # imports AGENTS.md so Claude Code and opencode read the same rules
TASKS.md               # the work queue + Log
README.md              # replace with the project's own
ADOPTION.md            # how to adopt this structure — template-only; delete after
.gitignore
.claude/skills/        # project skills, one folder each; ships grilling + grill-me
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
setup → idea → triage → plan → grill → tasks → feature doc
```

- **Setup** — before anything else, the agent asks which agent and model you use for UI (your frontier model) and for BACKEND (your cheap model), and writes them to `SETUP.md`. Any other role, e.g. `CAPTION`, is set up the same way the first time it is needed.

- **Idea** — the owner's words, captured in `docs/1-ideas/`, never built directly.
- **Triage** — owner or BACKEND sets a verdict: `build`, `park`, `drop`.
- **Plan** — `docs/2-plans/`, ends with its work split by lane.
- **Grill** — the plan is stress-tested with the `grilling` skill (`/grill-me`) before it is `reviewed`.
- **Tasks** — one row per item in `TASKS.md`, each owned by `UI`, `BACKEND`, an extra role from `SETUP.md`, or `OWNER` (human).
- **Done** — `Done when` observed, row flagged `[x]`, one line in the Log.

## Customising

The tools and models behind each lane live in `SETUP.md`; the lane names, the file-ownership table and the naming conventions live in `AGENTS.md`. Change them here once and every project created from the template inherits the change.
