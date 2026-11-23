# AGENTS.md

Project context for coding agents (Claude Code, opencode, etc.). Read this first; keep it small and evergreen.

## Project

**[FILL: project name]** — *[FILL: one-line pitch]*

**Stack:**
- [FILL: frontend]
- [FILL: backend / data]
- [FILL: deploy]

## Layout

- `SETUP.md` — which agent and model run each role in this repo. Read by Rule 0 before anything else.
- `src/` — [FILL: what lives here; if a folder needs narrower rules, give it its own `AGENTS.md` and link it here].
- `docs/` — single source of truth for product and strategy. **Read first when the question is "what should we build / how does it work / what does this term mean".** Structure: [`docs/README.md`](docs/README.md).
  - `docs/0-strategy.md` — concept, mechanics, playbook, risks, glossary.
  - `docs/1-ideas/` — raw ideas in the owner's words, before any plan (`I##-slug.md`).
  - `docs/2-plans/` — build scopes and plans.
  - `docs/3-features.md` — feature catalog (high level); detail per feature in `docs/3-features/<feature>/`.
- `.claude/skills/` — project skills, one folder each; read by Claude Code and opencode (Rule 6).

## Lanes — the frontier model builds UI, the cheap model builds the backend

Plans in `docs/` are specs. **Execution runs from [`TASKS.md`](TASKS.md)**, where every task has exactly one owner. Which agent and model run each lane is set in [`SETUP.md`](SETUP.md), never here.

| Lane | Who runs it | Owns |
|---|---|---|
| **UI** | the UI row in [`SETUP.md`](SETUP.md) (frontier model) | What a user sees and touches: [FILL: e.g. `src/components/**`, `src/screens/**`, styles, page copy, client-side state, animations]. |
| **BACKEND** | the BACKEND row in [`SETUP.md`](SETUP.md) (cheap model) | Data and infrastructure: [FILL: e.g. `worker/**`, `shared/**`, API routes, migrations, scripts, config, deploy]. |
| **OWNER** | the human | [FILL: e.g. curation, pricing, legal, accounts, secrets]. Agents never execute these. |
| **Extra roles** | their rows in [`SETUP.md`](SETUP.md) | What each row's **Owns** column says, e.g. `CAPTION` for captions and posts. |

`docs/**`, `TASKS.md`, `README.md`, `SETUP.md`, `AGENTS.md`, `CLAUDE.md`, and tests are shared: update the docs/tests for your own change; never rewrite the other lane's tests to make them pass. A new dependency or config change is BACKEND's call — file a request. The owner can override any rule; record the exception in the `TASKS.md` Log.

### Rule 0 — Setup (before Rule 1)

Read [`SETUP.md`](SETUP.md). Stop all work and run the setup interview with the [`grilling`](.claude/skills/grilling/SKILL.md) skill when any of these is true:

- a cell in `SETUP.md` still says `[FILL`;
- `git config user.name` is not the name in **Confirmed by** — a new person inherited someone else's models;
- the work needs a role that is not in `SETUP.md` (e.g. captions with no `CAPTION` row). Never hand it to UI or BACKEND instead.

The interview asks, per role: the agent, the model ID, the effort, the switch command, and for an extra role what it owns and which skill it must load. Write the answers into `SETUP.md`, set **Confirmed by** to the current `git config user.name` and today's date, log it in `TASKS.md`, then continue with Rule 1.

### Rule 1 — Pre-flight (before any work)

1. **Who am I?** Find the row in `SETUP.md` whose agent and model match the ones you are running on — that row's role is your lane. No row matches → you are nobody for task work; stop and say: "`<ROLE>` work runs on `<agent>` + `<model>`. Switch with `<switch command>`." Non-task work — capturing an idea, editing docs, answering a question — runs on any agent.
2. **Read `TASKS.md`.** If a task of yours is `[~]`, resume it.
3. **Otherwise take the first `[ ]` task, top to bottom, whose `Needs` are all `[x]`.**
4. **Owner check.** If it is not yours, **stop before touching code**:
   > Next task is `<ID> <title>`, owned by `<ROLE>`. Run it in `<agent>` on `<model>` (`<switch command>`).

   An `OWNER` task is a human task — say so and stop. Exception: a ready task of your own lane further down may run in parallel; say so, then take it.
5. **Announce** `Starting <ID> <title> as <ROLE>` and mark it `[~]`.

### Rule 2 — Stay in your lane

- Edit only files your lane owns. Never edit the other lane's files, even for a "quick fix".
- UI needs data, an endpoint, a field, or a BACKEND fix → add a line to [`docs/4-contract-requests.md`](docs/4-contract-requests.md), mock/continue, and let BACKEND clear it before its next task. Do not build the BACKEND side yourself.
- BACKEND needs a visual, copy, or interaction change → queue it as a `UI` task. Do not edit UI files.
- Work that belongs to an extra role → queue it as that role's task. If the role does not exist yet, Rule 0 sets it up first.
- A task that crosses lanes is split before it starts: BACKEND contract first, then the UI task with `Needs:` pointing at it.

### Rule 3 — Plans are grilled, then become tasks before code

A doc in `docs/` is a spec, not a work order.

- **Grill before review.** A plan in `docs/2-plans/` moves from `draft` to `reviewed` only after a grilling session with the owner (the [`grilling`](.claude/skills/grilling/SKILL.md) skill; the owner can type `/grill-me`). The plan then carries `**Grilled:** YYYY-MM-DD` and its **Decisions** table is filled from the session. No `Grilled` line → still `draft`.
- When the user says **execute**, **run**, or **build** a plan:
  1. Check the plan has a `**Grilled:**` date. If not, grill it first.
  2. Check `TASKS.md` has a task for every plan item you'll touch.
  3. If not, add them — one lane per task, with `Needs` and `Done when` — **before writing code**. A mixed item becomes a BACKEND task plus a UI task.
  4. Only then run the pre-flight and start.

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
- Triage belongs to the owner or BACKEND: verdict `build` → write a plan in `docs/2-plans/` and grill it → split it into tasks (Rule 3); `park`; or `drop`.
- An idea never jumps straight to code.

### Rule 6 — Skills live in one place

A skill is an optional how-to an agent loads on demand. It is not a rule: anything every agent must follow belongs in this file or in tests.

- **One folder:** `.claude/skills/<name>/SKILL.md`. Claude Code and opencode both read it, and it is committed. Never copy a skill into another tool's folder (`.opencode/skills/`, `.agents/skills/`). If a new agent reads somewhere else, point that folder here with a symlink.
- **Only skill folders in it.** Every subfolder is scanned as a skill, so no `_template/` folder; use the skeleton below.
- **Write for the strictest reader.** `name` equals the folder name (lowercase letters, digits, single hyphens; at most 64 characters). `description` says what it does and when to use it (at most 1024 characters, in quotes). `SKILL.md` stays under 500 lines; the rest goes in `references/` or `scripts/`. No tool-specific frontmatter such as `disable-model-invocation` in a skill written here.
- **Vendored skills stay verbatim.** A skill copied from another repo is kept byte-for-byte as upstream ships it, tool-specific keys included, and its source is listed below. Update it by copying the new upstream version, never by editing it here.
- **Lane-specific skills** still go in this folder; say the lane in the description.
- **Name the must-use skills.** Whether a skill triggers from its description varies by tool and model, so when a task must use one, say so here or in the task row ("use the `<name>` skill").
- **Done check.** After adding a skill, ask every agent in `SETUP.md` "what skills are available?". All must list it; if not, fix it before marking the task `[x]`.

**Must-use skills:**

| Skill | Source | Use it for |
|---|---|---|
| [`grilling`](.claude/skills/grilling/SKILL.md) | vendored, [`mattpocock/skills`](https://github.com/mattpocock/skills) `skills/productivity/grilling` | Every plan before `reviewed` (Rule 3) and every setup interview (Rule 0). Any agent, any role. |
| [`grill-me`](.claude/skills/grill-me/SKILL.md) | vendored, [`mattpocock/skills`](https://github.com/mattpocock/skills) `skills/productivity/grill-me` | The owner's `/grill-me` command; it only calls `grilling`. |

```md
---
name: example-skill
description: "What it does. Use when the user asks for X, Y, or Z."
---

# example-skill

Instructions.
```

### Rule 7 — Delegate inside the session

When a task needs another role's agent or model, look it up in `SETUP.md` and delegate it inside the current session with the tool's own delegation (a subagent, a delegate task, or a worker) so the brief, the work and the evidence stay together, and say so in the reply ("delegated inside this session/thread"). Before you hand off, check what the harness can spawn; hand off only when that role's agent or model is not reachable, and name the missing piece. Open a separate session, thread or worktree only when the owner asks for one; if you replace one, say the old one is closed. A delegated task keeps its owner lane — the delegating agent does not edit the other lane's files — and the delegation is recorded in the `TASKS.md` Log.

## Naming

One format for every role, so any agent reads any file. Based on the Google [JavaScript](https://google.github.io/styleguide/jsguide.html), [HTML/CSS](https://google.github.io/styleguide/htmlcssguide.html) and [JSON](https://google.github.io/styleguide/jsoncguide.html) style guides. Where a framework fixes a name (`page.tsx`, `index.ts`, `__init__.py`), the framework wins. New code follows this; existing names are fixed only when a task already touches the file.

| Thing | Format | Example |
|---|---|---|
| Task ID | `<ROLE><n>`, one counter per role | `UI3`, `BACKEND12`, `OWNER2`, `CAPTION1` |
| Files and folders | lowercase ASCII kebab-case, no spaces | `user-card.tsx`, `sync-orders.ts` |
| Ideas · plans · features | `I##-slug.md` · `2-plans/<slug>.md` · `3-features/<feature>/` | `I04-offline-mode.md` |
| Attachments | `assets/<what>-<variant>.<ext>` in the feature folder | `assets/checkout-flow-mobile.png` |
| **UI** — components, classes | UpperCamelCase | `UserCard` |
| **UI** — CSS classes | kebab-case, named for the role, not the look | `.error-status`, not `.red-text` |
| **UI** — HTML `id` | kebab-case, always with a hyphen | `user-profile` |
| **UI** — design tokens | kebab-case, `<group>-<name>` | `color-action`, `space-4` |
| **BACKEND** — API paths | lowercase kebab-case, plural nouns | `/order-items/{id}` |
| **BACKEND** — database tables, columns | snake_case, tables plural | `order_items.created_at` |
| **BACKEND** — env vars | UPPER_SNAKE | `DATABASE_URL` |
| Variables, functions, params | lowerCamelCase, no one-letter names | `drawFace`, `orderTotal` |
| Constants | UPPER_SNAKE at module level only; lowerCamelCase inside a function | `MAX_RETRIES` |
| JSON keys | lowerCamelCase; arrays plural, the rest singular; keys of a map object are free | `lineItems`, `createdAt` |

## Common commands

```bash
# [FILL: install / dev / test / build / deploy]
```

## Conventions

- **Source of truth for product decisions is `docs/`.** If code and `docs/` disagree, fix the code, then update `docs/`.
- **Keep `docs/` current with every change.** Don't let code land ahead of docs. A new feature → a `docs/3-features/<feature>/` doc plus a `3-features.md` entry; changed behavior → edit that feature doc (and its `**Status:**`); a new secret, env var, or ops step → the relevant feature doc.
- **Archive finished docs.** When a plan/feature doc is fully implemented (or only describes completed one-off work), move it to `docs/9-archive/` instead of deleting it. Evergreen docs (strategy, the feature catalog) stay in `docs/`.
- **Plan docs carry a status.** Every plan/feature doc has a status line under the title: `**Status:** draft | reviewed | done`. `draft` = written, not grilled; `reviewed` = grilled and agreed, not yet executed; `done` = executed. Only `done` docs get moved to `docs/9-archive/`.
- **Keep this file small.** If a rule only matters for one folder, put it in that folder's `AGENTS.md`. Only project-wide rules belong here.
- **No secrets in this file.** Environment variables live in `.env` (gitignored). See `.env.example` for the shape.
- **Don't add comments to code unless asked.** Match existing style.
- **No commits unless explicitly asked.**
