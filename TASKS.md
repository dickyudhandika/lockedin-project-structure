# TASKS — [FILL: project name]

Execution queue. Rules: [`AGENTS.md`](AGENTS.md). Specs: [`docs/`](docs/README.md).
Take the first `[ ]` task, top to bottom, whose **Needs** are all `[x]`. If its owner is not your lane, stop and hand off (Rule 1 in [`AGENTS.md`](AGENTS.md)). `Needs` ranges are inclusive.

`[ ]` todo · `[~]` in progress · `[x]` done · `[!]` blocked

**Lanes:** `UI` = Claude Code · `BACKEND` = opencode · `OWNER` = human.

**Queue source:** [FILL: the plan this queue is split from, e.g. `docs/2-plans/mvp-scope.md`].

Row format:

```
- [ ] **<ID>** · <LANE> · <what to do> · Needs: <IDs or —> · Done when: <observable check>
```

---

## Phase 1 — Setup

- [ ] **OWNER1** · OWNER · Fill the blanks: pitch, stack, and the lane ownership table in `AGENTS.md`; the bet and rules in `docs/0-strategy.md`; the title and **Queue source** in `TASKS.md`; replace `README.md` with the project's own. In an existing repo, answer from the code — see [`ADOPTION.md`](ADOPTION.md) step 3. · Needs: — · Done when: `grep -rn "\[FILL" AGENTS.md docs/` returns nothing, `README.md` describes the project rather than the template, and `ADOPTION.md` is deleted.
- [ ] **BACKEND1** · BACKEND · Scaffold the repo for the stack in `AGENTS.md`; make dev, test, and build run; record the commands under "Common commands". · Needs: OWNER1 · Done when: the dev server starts, one test passes, and the commands in `AGENTS.md` are the ones that worked.
- [ ] **UI1** · UI · First screen / app shell, styled with the project's tokens. · Needs: BACKEND1 · Done when: the shell renders and the flow named in the plan can be walked by hand.

## Parked — not blocking

_none_

## Log

- YYYY-MM-DD SETUP OWNER: Project created from the project-structure template. First ready task: **OWNER1**.
