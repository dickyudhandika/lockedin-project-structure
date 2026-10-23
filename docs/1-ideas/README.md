# Ideas — the inbox before a plan

Raw ideas, requests, and complaints, kept in the owner's words. An idea says *what hurts or what might be good*; it does not say how to build it. Nothing here is a commitment and nothing here is a task.

## Files

- One idea per file: `I##-short-slug.md` — `I01-offline-mode.md`, `I02-dark-theme.md`.
- IDs are sequential and never reused, even when an idea is dropped. Find the next one with `ls docs/1-ideas`.
- Start from [`_template.md`](_template.md). Fill everything except **Triage**.

## Status

The marker sits under the title, in the file itself — there is no separate index to keep in sync.

| Marker | Meaning |
|---|---|
| `[ ] raw` | Captured, not yet looked at. |
| `[~] triaged` | A verdict is recorded. May be waiting (`park`) or in flight (`build`). |
| `[x] done` | Shipped; the plan and task IDs are in **Triage**. |
| `[-] dropped` | Decided against; **Why** says why, so it is not re-proposed. |

See where everything stands: `grep -H "Status:" docs/1-ideas/I*.md`

## Flow

```
capture  →  triage         →  plan          →  tasks     →  done
anyone       owner/BACKEND     docs/2-plans     TASKS.md
```

1. **Capture.** Anyone — human or agent — writes the file with status `[ ] raw`. Keep the owner's exact words under **Said**. Write the problem; leave the solution open. Do not plan or build.
2. **Triage.** The owner or BACKEND reads the idea and fills **Triage**:
   - **Verdict** — `build`, `park`, or `drop`.
   - **Why** — one or two lines.
   - **Plan doc** — the `docs/2-plans/` file, once there is one.
   - **Task IDs** — the `TASKS.md` rows split from that plan.
   - **Decided on** — the date.
3. **Plan.** A `build` verdict gets a doc in [`../2-plans/`](../2-plans/) that links back to the idea.
4. **Tasks.** The plan is split by lane into [`TASKS.md`](../../TASKS.md) (Rule 3 in `AGENTS.md`). The idea becomes `[x] done` when those tasks are `[x]`.

Done and dropped ideas stay in this folder: plans and the task Log refer to them by ID, so moving them would break the trail.

## Writing a good idea

- **Problem before solution.** Describe what happens today and who it hurts. If the owner floated a solution, record it under **Idea as told** and mark the choice as unresolved.
- **Open questions are the point.** The questions you cannot answer yet are what triage works from.
- **Name what is out of scope** if an earlier decision already ruled something out, and link the doc that decided it.
