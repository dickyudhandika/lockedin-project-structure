# Adopting this structure

> **Template-only file.** It describes how to install the structure, not a product. Delete it once your queue is running (see [Both paths end here](#both-paths-end-here)). `OWNER1` in `TASKS.md` includes that deletion.

Two ways in, one end state. Pick the row that matches today:

| You have | Go to | The work is |
|---|---|---|
| No repo, or an empty one | [Starting from zero](#starting-from-zero) | Scaffold, fill intent, then build |
| A repo with commits, users, or a backlog | [Retrofitting an existing project](#retrofitting-an-existing-project) | Merge rules, then backfill intent from what ships |

Greenfield is a straight line. A retrofit runs the same pipeline in reverse for one pass — inventory, rules, facts, queue — and only then moves forward. The trap in a retrofit is treating it as a migration project: moving every old doc and freezing the repo. Don't.

## Starting from zero

1. **Create the repo from the template.** On GitHub, **Use this template → Create a new repository** — that gives you a clean history. Without GitHub:

   ```bash
   npx degit <you>/lockedin-project-structure my-project
   cd my-project && git init
   ```

2. **Fill the blanks before you start building.** `grep -rn "\[FILL" AGENTS.md docs/` lists them — `AGENTS.md` (pitch, stack, lanes, layout), `docs/0-strategy.md` (bet, rules, gates, risks, glossary) — plus the title and **Queue source** in `TASKS.md`.

3. **Name the lanes against folders that will exist**, not folders you hope to have. If one person and one agent do everything, collapse `UI` and `BACKEND` into one lane now; splitting later is cheap, unwinding two half-owned folders is not.

4. **Run the queue.** `OWNER1` first, then `BACKEND1` scaffolds and confirms the stack and commands written in step 2 were real. If they were guesses, correct `AGENTS.md` in the same commit.

5. **Then write the first idea.** Even a decision you have already made goes through `docs/1-ideas/` → `docs/2-plans/` → `TASKS.md`. The point is not ceremony; it is that the plan, not memory, holds the scope.

If you cannot say who the product is for and the one job it does, you are not ready for `BACKEND1`. Time spent in `0-strategy.md` is cheaper than a scaffold you rebuild.

## Retrofitting an existing project

Order matters: install files, merge rules, fill facts from the code, then point the queue at reality. Do not start by moving your docs.

### 1. Copy only what you lack

Clone the template outside your repo and copy in missing files, overwriting nothing:

```bash
git clone --depth 1 <template-url> /tmp/lockedin
rsync -av --ignore-existing \
  --exclude .git --exclude README.md --exclude .gitignore --exclude .DS_Store \
  /tmp/lockedin/ ./
```

That lands `AGENTS.md`, `CLAUDE.md`, `TASKS.md`, `docs/`, and `.claude/skills/`. It leaves your `README.md`, your `.gitignore`, and every same-named file alone. Merge the `.gitignore` groups you are missing (secrets, OS noise, agent state) by hand; keep your build-output rules.

**If you already have a `docs/` folder, do not nest a second one.** Two options:

- **Move it in.** Strategy facts → `0-strategy.md`, undecided questions → `1-ideas/`, live scopes → `2-plans/`, shipped features → `3-features/`, everything historical → `9-archive/`. Best when your docs are a loose pile.
- **Keep your root.** If those docs are published or load-bearing, leave them and point the **Layout** and **Docs** bullets in `AGENTS.md` at your existing root. The numbered folders then live inside it. One root, named in one place.

### 2. Merge agent rules, never replace them

- **`AGENTS.md` already exists** → keep its project facts (stack, commands, conventions) and splice in the template's **Lanes** table, **Rules 1–6**, and status markers. The rules assume `TASKS.md` and `docs/` exist; copy those first.
- **Only `CLAUDE.md`, `.cursorrules`, `.github/copilot-instructions.md`, or `GEMINI.md` exist** → make `AGENTS.md` canonical and reduce the others to pointers (`@AGENTS.md` in `CLAUDE.md`, a one-line "see AGENTS.md" elsewhere). Two copies of the rules is the failure mode; they drift within a week.
- **Monorepo with per-folder instructions** → keep them, link each from **Layout**, and let the root `AGENTS.md` stay project-wide. Do not flatten folder rules into the root.

### 3. Fill the blanks from the repo, not from imagination

| Blank | Where the true answer already lives |
|---|---|
| Stack, commands | `package.json`, `Makefile`, CI workflow. Run each command once; edit the doc if it fails. |
| Lane ownership | Actual top-level folders. Split by what a change touches, and give every folder exactly one lane. |
| The bet, **Who** | Landing page, pitch deck, the last thing you told a user. |
| Rules of the game | Invariants already enforced in code: auth, tenancy, pricing, limits, data retention. |
| Stages and gates | What already shipped, and the metric you are currently waiting on. |
| Risks and kill criteria | Open issues, incidents, the thing you keep postponing. |
| Glossary | Domain words used in the codebase, including the ones that mean two things. |

Timebox this to one sitting. A true strategy doc with `TBD` in three cells beats a complete one invented in an afternoon — mark each `TBD` with an `OWNER` task so it does not rot.

### 4. Seed the queue with your reality

Do not paste a greenfield plan over a repo that already ships. Rewrite the first rows:

| Template row | In an existing project |
|---|---|
| `OWNER1` fill blanks | Keep it, answer from step 3, and add "delete `ADOPTION.md`" to **Done when**. |
| `BACKEND1` scaffold the repo | Replace with **baseline**: dev, test, and build run exactly as documented. |
| `UI1` app shell | Replace with a real screen or flow that already exists, or drop the row. |
| — | Add a `BACKEND`/`UI` task: **baseline feature docs** — one doc per shipped feature, status `built`, one catalog line each. |

Then give every piece of work already in flight a row: open PRs, half-built branches, the thing living in someone's head. Let pre-existing PRs finish untouched; from the next change on, work starts as a task row. Baseline docs are the highest-value retrofit task — they are what makes the structure pay for itself on day one.

### 5. Point the pipeline forward and backfill lazily

- **New request → idea file,** even when it is small and already agreed. One line under **Said** is enough; triage happens in the same sitting if you want.
- **New build → plan → tasks.** A one-file fix can be a single task with no plan. The rule that always binds is one lane per task and a checkable **Done when**.
- **Backfill one feature each time you touch it.** Open the code, write the doc while the context is loaded, add the catalog entry. Never stop shipping for a docs sprint.
- **Expect docs to trail code for the first weeks.** A stale doc you fix later is recoverable; a repo frozen for a docs migration usually ends with neither docs nor momentum.

### 6. Land it in small commits

1. Structure only: `AGENTS.md`, `TASKS.md`, `docs/` skeleton, skills.
2. Blanks filled from step 3.
3. Queue seeded from step 4.
4. Baseline feature docs.

Each commit is reviewable alone, and stopping after any of them still leaves a working repo. Never bundle the retrofit with a product change.

## Both paths end here

```text
[ ] grep -rn "\[FILL" AGENTS.md docs/ returns nothing
[ ] TASKS.md title and Queue source are filled
[ ] AGENTS.md lanes match real folders, and every folder has one lane
[ ] dev / test / build commands were copied from CI and run once
[ ] docs/0-strategy.md describes the product that exists (or will exist)
[ ] the first ready row in TASKS.md is real work, not a placeholder
[ ] docs/3-features.md matches what actually ships
[ ] README.md describes the project, not the template
[ ] ADOPTION.md deleted
```
