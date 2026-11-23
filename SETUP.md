# SETUP — who runs each role

Every person runs this project with different tools. This file records which coding agent and which model run each role in **this** repo. It is the only place a model or agent is named; every other doc says "the UI role" or "the BACKEND role" and reads the details from here.

**Confirmed by:** [FILL: `git config user.name`], [FILL: YYYY-MM-DD]

Rule 0 in [`AGENTS.md`](AGENTS.md) runs the setup interview with the [`grilling`](.claude/skills/grilling/SKILL.md) skill whenever a cell below says `[FILL`, the person at the keyboard is not the one in **Confirmed by**, or the work needs a role that is not listed here.

## Required roles

| Role | Job | Agent | Model ID | Effort | Switch command |
|---|---|---|---|---|---|
| **UI** | Frontier model. What a user sees and touches. | [FILL: e.g. Claude Code] | [FILL: e.g. `claude-opus-5-5`] | [FILL: e.g. high] | [FILL: e.g. `/model claude-opus-5-5`] |
| **BACKEND** | Cheap model. Data, infrastructure, scripts, config. | [FILL: e.g. opencode] | [FILL: e.g. `ollama-cloud/minimax-m3`] | [FILL: e.g. — ] | [FILL: e.g. `opencode -m ollama-cloud/minimax-m3`] |

What each role owns, file by file, is the **Lanes** table in [`AGENTS.md`](AGENTS.md). One person with one agent can put the same agent and model in both rows; the lanes still stay separate.

## Extra roles

_none_

Any work that is not UI, BACKEND or OWNER needs its own role here **before** any agent does it. There is no fallback: a caption is not BACKEND work just because BACKEND is free. Example of a filled row:

| Role | Owns | Must-use skill | Agent | Model ID | Effort | Switch command |
|---|---|---|---|---|---|---|
| **CAPTION** | Captions, cover lines, posts, on-screen words | `<copy-skill>` | opencode | `ollama-cloud/minimax-m3` | — | `opencode -m ollama-cloud/minimax-m3` |

A role name is one uppercase word. Its tasks are numbered like any lane (`CAPTION1`, `CAPTION2`, …), and Rule 1 checks the running agent and model against its row.
