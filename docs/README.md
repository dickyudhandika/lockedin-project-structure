# Docs — how this folder works

Single source of truth for product and strategy. If code and `docs/` disagree, fix the code, then update `docs/`.

## Map

```
docs/
  1-ideas/               # raw ideas in the owner's words, before any plan
  2-plans/               # build scopes and plans
  3-features/            # one folder per feature: detail docs + attachments
  9-archive/             # finished one-off docs (status: done)
  0-strategy.md          # evergreen product truth — concept, mechanics, playbook
  3-features.md          # feature catalog (high level, one entry per feature)
  4-contract-requests.md # UI → BACKEND requests (lane workflow)
  README.md              # this file — how docs work
```

The numbers are the pipeline stage, shared by folders and files: `0` strategy → `1` ideas → `2` plans → `3` features (the catalog `3-features.md` sits beside its `3-features/` folder) → `4` live contract requests → `9` archive. Explorers list folders before files, so the numbers order each group. `README.md` stays unnumbered so GitHub renders it. Use `5-`…`8-` for anything new that belongs between building and archiving.

Start at [`0-strategy.md`](0-strategy.md) for *what and why*. Feature detail lives in [`3-features/`](3-features/) — the catalog is [`3-features.md`](3-features.md). Anything not yet decided starts in [`1-ideas/`](1-ideas/README.md).

## The pipeline

```
idea      →  triage     →  plan      →  tasks     →  feature doc
1-ideas/     (verdict)     2-plans/     TASKS.md    3-features/ + 3-features.md
```

Each step has one home. An idea is never a plan, a plan is never a work order, and a work order (`TASKS.md`) is never a spec.

## Conventions

- **Status line.** Every plan/feature doc carries `**Status:** draft | reviewed | done` under the title. `draft` = written, not reviewed · `reviewed` = plan agreed, not yet executed · `done` = executed. Ideas use their own markers — see [`1-ideas/README.md`](1-ideas/README.md).
- **Archive finished docs.** When a plan/feature doc is fully implemented (or only describes completed one-off work), move it to `9-archive/` instead of deleting it. Evergreen docs (strategy, the feature catalog) never archive.
- **`3-features.md` is a map, not a dump.** One short entry per feature — name, one-liner, how it works, status, link. Detail lives only in the feature folder.
- **Feature folders own their attachments.** Images, diagrams, and exports go in the feature folder's `assets/`, never in a central pile. Start a new feature by copying [`3-features/_template/`](3-features/_template/).
- **Plans split by lane before execution.** Every build plan ends with its work split: `UI` (Claude Code), `BACKEND` (opencode), `OWNER` (human). Execution status lives only in [`TASKS.md`](../TASKS.md) — a plan stays `reviewed` until its tasks are `[x]`.
- **Strategy supersedes.** Where docs conflict, `0-strategy.md` decides once the question is answered; other docs follow it.
