# AGENTS.md

Project context for coding agents (Claude Code, opencode, etc.). Read this first; keep it small and evergreen.

## Project

**[FILL: project name]** — *[FILL: one-line pitch]*

**Stack:**
- [FILL: frontend]
- [FILL: backend / data]
- [FILL: deploy]

## Layout

- `src/` — [FILL: what lives here; if a folder needs narrower rules, give it its own `AGENTS.md` and link it here].
- `docs/` — single source of truth for product and strategy. **Read first when the question is "what should we build / how does it work / what does this term mean".** Structure: [`docs/README.md`](docs/README.md).
  - `docs/0-strategy.md` — concept, mechanics, playbook, risks, glossary.
  - `docs/1-ideas/` — raw ideas in the owner's words, before any plan (`I##-slug.md`).
  - `docs/2-plans/` — build scopes and plans.
  - `docs/3-features.md` — feature catalog (high level); detail per feature in `docs/3-features/<feature>/`.
- `.claude/skills/` — project skills, one folder each; read by Claude Code and opencode (Rule 6).

## Lanes — Claude Code builds UI, opencode builds the backend

Plans in `docs/` are specs. **Execution runs from [`TASKS.md`](TASKS.md)**, where every task has exactly one owner.

| Lane | Who runs it | Owns |
|---|---|---|
| **UI** | **Claude Code** | What a user sees and touches: [FILL: e.g. `src/components/**`, `src/screens/**`, styles, page copy, client-side state, animations]. |
| **BACKEND** | **opencode** | Data and infrastructure: [FILL: e.g. `worker/**`, `shared/**`, API routes, migrations, scripts, config, deploy]. |
| **OWNER** | the human | [FILL: e.g. curation, pricing, legal, accounts, secrets]. Agents never execute these. |

`docs/**`, `TASKS.md`, `README.md`, `AGENTS.md`, `CLAUDE.md`, and tests are shared: update the docs/tests for your own change; never rewrite the other lane's tests to make them pass. A new dependency or config change is BACKEND's call — file a request. The owner can override any rule; record the exception in the `TASKS.md` Log.

### Rule 1 — Pre-flight (before any work)

1. **Who am I?** Claude Code → **UI**. opencode → **BACKEND**. Any other agent → stop; this project runs UI in Claude Code and BACKEND in opencode.
2. **Read `TASKS.md`.** If a task of yours is `[~]`, resume it.
3. **Otherwise take the first `[ ]` task, top to bottom, whose `Needs` are all `[x]`.**
4. **Owner check.** If it is not yours, **stop before touching code**:
   > Next task is `<ID> <title>`, owned by `<UI (Claude Code) | BACKEND (opencode)>`. Run it in `<Claude Code | opencode>`.

   An `OWNER` task is a human task — say so and stop. Exception: a ready task of your own lane further down may run in parallel; say so, then take it.
5. **Announce** `Starting <ID> <title> as <UI|BACKEND>` and mark it `[~]`.

### Rule 2 — Stay in your lane

- Edit only files your lane owns. Never edit the other lane's files, even for a "quick fix".
- UI needs data, an endpoint, a field, or a BACKEND fix → add a line to [`docs/4-contract-requests.md`](docs/4-contract-requests.md), mock/continue, and let BACKEND clear it before its next task. Do not build the BACKEND side yourself.
- BACKEND needs a visual, copy, or interaction change → queue it as a `UI` task. Do not edit UI files.
- A task that crosses lanes is split before it starts: BACKEND contract first, then the UI task with `Needs:` pointing at it.

### Rule 3 — Plans become tasks before code

A doc in `docs/` is a spec, not a work order. When the user says **execute**, **run**, or **build** a plan, start here:

1. Check `TASKS.md` has a task for every plan item you'll touch.
2. If not, add them — one lane per task, with `Needs` and `Done when` — **before writing code**. A mixed item becomes a BACKEND task plus a UI task.
3. Only then run the pre-flight and start.

A plan stays `**Status:** reviewed` until all its tasks are `[x]`; then mark it `done` (and archive it unless it's evergreen).

### Rule 4 — Flag done so the next task can start

Done means the task's **Done when** is true (tests pass, flow verified, evidence in the Log). Then, in the same session:

1. `[~]` → `[x]` in `TASKS.md`, append `— done YYYY-MM-DD`.
2. Add one line to the **Log**: `YYYY-MM-DD <ID> <LANE>: <what changed, what the other lane must know>`.
3. Run Rule 1 again. Yours → continue. Other lane → stop with the handoff message.

Never mark `[x]` if checks fail. Blocked outside your lane → `[!]` with the reason and stop.

**Status markers:** `[ ]` todo · `[~]` in progress · `[x]` done · `[!]` blocked.

### Rule 5 — Ideas are captured, not built

A new idea, request, or complaint from the owner goes into [`docs/1-ideas/`](docs/1-ideas/README.md) as the next `I##-slug.md` (copy [`_template.md`](docs/1-ideas/_template.md)), status `[ ] raw`, with the owner's words kept verbatim under **Said**.

- Capture only. Do not plan, design, or code it, and do not add it to `TASKS.md`.
- Triage belongs to the owner or BACKEND: verdict `build` → write a plan in `docs/2-plans/` → split it into tasks (Rule 3); `park`; or `drop`.
- An idea never jumps straight to code.

### Rule 6 — Skills live in one place

A skill is an optional how-to an agent loads on demand. It is not a rule: anything every agent must follow belongs in this file or in tests.

- **One folder:** `.claude/skills/<name>/SKILL.md`. Claude Code and opencode both read it, and it is committed. Never copy a skill into another tool's folder (`.opencode/skills/`, `.agents/skills/`). If a new agent reads somewhere else, point that folder here with a symlink.
- **Only skill folders in it.** Every subfolder is scanned as a skill, so no `_template/` folder; use the skeleton below.
- **Write for the strictest reader.** `name` equals the folder name (lowercase letters, digits, single hyphens; at most 64 characters). `description` says what it does and when to use it (at most 1024 characters, in quotes). `SKILL.md` stays under 500 lines; the rest goes in `references/` or `scripts/`. No tool-specific frontmatter such as `disable-model-invocation`.
- **Lane-specific skills** still go in this folder; say the lane in the description.
- **Name the must-use skills.** Whether a skill triggers from its description varies by tool and model, so when a task must use one, say so here or in the task row ("use the `<name>` skill").
- **Done check.** After adding a skill, ask Claude Code and opencode "what skills are available?". Both must list it; if not, fix it before marking the task `[x]`.

```md
---
name: example-skill
description: "What it does. Use when the user asks for X, Y, or Z."
---

# example-skill

Instructions.
```

## Common commands

```bash
# [FILL: install / dev / test / build / deploy]
```

## Conventions

- **Source of truth for product decisions is `docs/`.** If code and `docs/` disagree, fix the code, then update `docs/`.
- **Keep `docs/` current with every change.** Don't let code land ahead of docs. A new feature → a `docs/3-features/<feature>/` doc plus a `3-features.md` entry; changed behavior → edit that feature doc (and its `**Status:**`); a new secret, env var, or ops step → the relevant feature doc.
- **Archive finished docs.** When a plan/feature doc is fully implemented (or only describes completed one-off work), move it to `docs/9-archive/` instead of deleting it. Evergreen docs (strategy, the feature catalog) stay in `docs/`.
- **Plan docs carry a status.** Every plan/feature doc has a status line under the title: `**Status:** draft | reviewed | done`. `draft` = written, not reviewed; `reviewed` = plan agreed, not yet executed; `done` = executed. Only `done` docs get moved to `docs/9-archive/`.
- **Keep this file small.** If a rule only matters for one folder, put it in that folder's `AGENTS.md`. Only project-wide rules belong here.
- **No secrets in this file.** Environment variables live in `.env` (gitignored). See `.env.example` for the shape.
- **Don't add comments to code unless asked.** Match existing style.
- **No commits unless explicitly asked.**
