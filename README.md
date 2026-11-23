<div align="center">
  <!-- TODO: hero image pending. Drop it at docs/public/readme-hero.png and replace this comment with:
  <img src="docs/public/readme-hero.png" alt="lockedin — Structure for everyone." />
  -->

  <h1>lockedin-project-structure</h1>

  <h3>A project template for teams that build with coding agents</h3>

  <p>
    Who runs what, one task queue, and a docs tree that keeps ideas, plans, and features apart.<br />
    Works with new and existing repos, and with any agent that reads <code>AGENTS.md</code>.
  </p>

  <p>
    <strong>One lane per task</strong>
    &nbsp;·&nbsp;
    <strong>Ideas never jump to code</strong>
    &nbsp;·&nbsp;
    <strong>MIT</strong>
  </p>

  <p>
    <a href="https://github.com/dickyudhandika/lockedin-project-structure/stargazers"><img src="https://img.shields.io/github/stars/dickyudhandika/lockedin-project-structure" alt="GitHub stars" /></a>
    <a href="https://github.com/dickyudhandika/lockedin-project-structure/commits/main"><img src="https://img.shields.io/github/last-commit/dickyudhandika/lockedin-project-structure" alt="Last commit" /></a>
    <a href="LICENSE"><img src="https://img.shields.io/github/license/dickyudhandika/lockedin-project-structure" alt="License" /></a>
  </p>

  <p>
    <a href="docs/README.md">📖 Docs</a>
    &nbsp;·&nbsp;
    <a href="#quick-start">🚀 Quick Start</a>
    &nbsp;·&nbsp;
    <a href="ADOPTION.md">🔁 Adopt in an existing repo</a>
    &nbsp;·&nbsp;
    <a href="AGENTS.md">📏 Rules</a>
  </p>
</div>

> **This file describes the template.** In a new project, replace it with the project's own README (task `OWNER1` in `TASKS.md`).

---

## Why this structure?

Coding agents are fast, and they drift. Left alone they edit each other's files, build ideas nobody agreed on, and forget which model was meant to do what.

- **The frontier model** is spent on UI work, and **the cheap model** on backend work. Nobody records which is which.
- **Ideas turn into code** the moment someone types them into a chat. The plan lives in memory, then disappears.
- **A new teammate** inherits another person's models, tools, and half-finished tasks.
- **Docs and code** disagree, and nobody knows which one to trust.

Built from real projects, not from a whiteboard.

| Without a structure | The catch |
|---|---|
| One agent does everything | The expensive model writes config, the cheap one touches the UI, and reviews get noisy |
| Plans in chat | Scope is whatever the last message said |
| Ad hoc docs | Strategy, ideas, and feature specs end up in one pile |
| **This template** | Every task has one owner, every plan is grilled before it is built, and `docs/` stays the source of truth |

## How it works

The template connects three parts:

1. **`SETUP.md`**: records which agent and model run each role (UI, BACKEND, and any extra role). The agent reads it first and runs a setup interview when a cell is blank.
2. **`AGENTS.md` + `TASKS.md`**: the rules and the queue. Each task has exactly one owner lane, and handoffs between lanes are explicit.
3. **`docs/`**: ideas, plans, and features, each in its own folder with its own status.

```text
setup → idea → triage → plan → grill → tasks → feature doc
```

- **Setup**: before anything else, the agent asks which agent and model you use for UI (your frontier model) and for BACKEND (your cheap model), and writes them to `SETUP.md`. Any other role, e.g. `CAPTION`, is set up the same way the first time it is needed.
- **Idea**: the owner's words, captured in `docs/1-ideas/`, never built directly.
- **Triage**: owner or BACKEND sets a verdict: `build`, `park`, `drop`.
- **Plan**: `docs/2-plans/`, ends with its work split by lane.
- **Grill**: the plan is stress-tested with the `grilling` skill (`/grill-me`) before it is `reviewed`.
- **Tasks**: one row per item in `TASKS.md`, each owned by `UI`, `BACKEND`, an extra role from `SETUP.md`, or `OWNER` (human).
- **Done**: `Done when` observed, row flagged `[x]`, one line in the Log.

## What this is not

It is not a framework, a CLI, or an agent. There is nothing to install and nothing to run: it is a set of files that agents and people read. It does not pick your stack, your agent, or your models; `SETUP.md` and `AGENTS.md` record the choices you make.

## Quick Start

### 1. Create the repo

On GitHub, **Use this template → Create a new repository**, then clone it. Without GitHub:

```sh
npx degit dickyudhandika/lockedin-project-structure my-project
cd my-project && git init
```

### 2. Open your coding agent

Open Claude Code, opencode, or any agent that reads `AGENTS.md`. Rule 0 starts the setup interview and fills `SETUP.md` with your UI and BACKEND agents and models.

### 3. Fill the blanks

```sh
grep -rn "\[FILL" SETUP.md AGENTS.md docs/
```

This lists the pitch, stack, lane ownership, and strategy cells that are still empty.

### 4. Work the queue

Start with `OWNER1` in `TASKS.md`, then `BACKEND1` to scaffold the stack. Every later idea goes through `docs/1-ideas/` → `docs/2-plans/` → `TASKS.md` before any code.

> **Already have a repo?** Copy the files in without overwriting yours and merge the rules into your `AGENTS.md`. Both paths, in order, are in [`ADOPTION.md`](ADOPTION.md). Delete it once your queue is running.

## Requirements

| Component | Requirements |
|-----------|-------------|
| **Repo** | Any git repo, new or existing |
| **Coding agent** | One that reads `AGENTS.md` and `.claude/skills/`, such as Claude Code or opencode |
| **Models** | One frontier model for UI and one cheaper model for BACKEND (they can be the same model) |
| **Owner** | A human for `OWNER` tasks: accounts, secrets, pricing, curation |

## What's included

- **Setup gate**: the agent confirms who runs each role before it touches anything, and re-asks when a new person inherits the repo.
- **Lanes**: UI and BACKEND own different files; cross-lane needs go through `docs/4-contract-requests.md`.
- **One task queue**: `TASKS.md` with status markers `[ ]` `[~]` `[x]` `[!]`, a `Needs` chain, and a Log.
- **Ideas inbox**: `docs/1-ideas/`, owner's words verbatim, captured and never built directly.
- **Grilled plans**: a plan is `draft` until a grilling session gives it a `Grilled` date.
- **Skills**: `.claude/skills/` ships `grilling` and `grill-me`; one folder per skill, read by every agent.
- **Naming conventions**: one format for tasks, files, CSS, API paths, tables, env vars, and JSON keys.
- **Delegation inside the session**: when a task needs another role's model, the agent delegates in place instead of opening a new session.

### What you get

```text
SETUP.md               # which agent + model run each role, filled per person, first
AGENTS.md              # rules: setup, lanes, pre-flight, handoff, ideas, skills, naming
CLAUDE.md              # imports AGENTS.md so Claude Code and opencode read the same rules
TASKS.md               # the work queue + Log
README.md              # replace with the project's own
ADOPTION.md            # how to adopt this structure; template-only, delete after
.gitignore
.claude/skills/        # project skills, one folder each; ships grilling + grill-me
docs/
  1-ideas/               # raw ideas, I##-slug.md   (README + _template)
  2-plans/               # build scopes             (_template)
  3-features/            # one folder per feature   (_template/ with assets/)
  9-archive/             # finished one-off docs
  0-strategy.md          # evergreen: the bet, rules, gates, glossary
  3-features.md          # catalog, one short entry per feature
  4-contract-requests.md # UI → BACKEND asks
  README.md              # how docs work + conventions
```

## Rules at a Glance

The full text is in [`AGENTS.md`](AGENTS.md).

| Rule | What it says |
|------|--------------|
| **0 · Setup** | Read `SETUP.md`. Blank cell, new person, or missing role → run the setup interview first |
| **1 · Pre-flight** | Find your lane, read `TASKS.md`, take the first ready task, stop if it is not yours |
| **2 · Stay in your lane** | Edit only your lane's files; cross-lane needs become a contract request or a queued task |
| **3 · Plans are grilled** | A plan becomes tasks, one lane each, before any code |
| **4 · Flag done** | `[x]`, a dated line in the Log, then run pre-flight again |
| **5 · Ideas are captured** | Into `docs/1-ideas/`, never built directly |
| **6 · Skills live in one place** | `.claude/skills/<name>/SKILL.md`, nowhere else |
| **7 · Delegate inside the session** | Use the harness's own delegation; open a new session only when asked |

## Documentation

- **Start here**: [`docs/README.md`](docs/README.md) · [`docs/0-strategy.md`](docs/0-strategy.md) · [`docs/3-features.md`](docs/3-features.md)
- **Process**: [`AGENTS.md`](AGENTS.md) · [`SETUP.md`](SETUP.md) · [`TASKS.md`](TASKS.md) · [`ADOPTION.md`](ADOPTION.md)
- **Skills**: [`grilling`](.claude/skills/grilling/SKILL.md) · [`grill-me`](.claude/skills/grill-me/SKILL.md)

## Customising

The tools and models behind each lane live in `SETUP.md`; the lane names, the file-ownership table and the naming conventions live in `AGENTS.md`. Change them here once and every project created from the template inherits the change.

## Contributing

Follow the rules in [`AGENTS.md`](AGENTS.md): capture ideas in `docs/1-ideas/`, plan in `docs/2-plans/`, and update the docs with your change.

## License

[MIT](LICENSE) — Copyright © 2026 Dicky
