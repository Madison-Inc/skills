# Madison skills

Skills that let your agent work [Madison](https://madison.works) tickets the way a careful
teammate would: write a ticket up, design it, pick it up, explore the code behind it, get a second opinion, build,
check and ship it your team's way, and write every decision and finding back onto the
ticket so the *why* outlives the session, including when you hand the work to a new one.

| Skill | When to use it |
| --- | --- |
| `setup-madison` | Whenever you want to see or change how your team works with Madison (when a ticket is Done, which agent does what, your ticket rules, …), or answer several setup questions at once. You never need it first: each skill asks a blank answer when it needs it. |
| `create-problem` | Something is going wrong for a user or the business: writes it up as a Problem (the pain, never the fix), replies with its link, then works its open questions with you. |
| `create-bug` | Something is broken: writes it up as a Bug with how to reproduce it, and replies with its link. |
| `create-initiative` | Choosing how to solve a Problem: writes the direction up as an Initiative linked to it, then works its open questions with you, including whether an MVP comes first. |
| `create-design` | An Initiative changes what users see and has no design yet: makes the design in your team's design tool from what the ticket says, and sets the Initiative's design link so every Task under it is handed the design. Revises an existing design in place. Only Claude Design is supported; for any other tool it asks you to make the design and sets the link you give. `create-initiative`, `start-ticket` and `run-spike` hand off to it. |
| `run-spike` | An open question on an Initiative or Task needs evidence, not a choice: answers it with research or throwaway code (kept outside your project) as a Task labelled `spike`, then writes each answer onto the ticket as a finding, or a decision when you make a choice with it, once you approve. `start-ticket`, `create-initiative` and `ship` hand off to it. |
| `create-task` | Planning the work for an Initiative: writes a Task with a user story, linked to it. When the Initiative builds an MVP first, only the MVP Task until it passes. |
| `review-tasks` | After writing Tasks, before building them: a second opinion that checks each Task against its Initiative's decisions, whether its acceptance criteria can be tested, and the Task rules, then writes the fixes and posts the review once you approve. Give it an Initiative to review all its open Tasks, or one Task. |
| `start-ticket` | Picking up a ticket: checks it is still relevant, explains it plainly, and works its open questions with you one at a time. |
| `still-relevant` | Wondering whether an open ticket is still needed: checks it against the code, other tickets and its parents' decisions, proposes Done, Closed or keep with the evidence, and records it once you approve. One ticket at a time. |
| `explore` | Before building: reads the code against the ticket and posts what it found, plus the decisions still open, as a comment. |
| `review-comment` | When another agent has left a comment (an exploration, a review): explains it with a recommendation per decision, then posts your answers once you approve. |
| `build` | Once a ticket is decided: sets it In Progress and implements it, branching, reviewing and raising the change the way your own agent files (CLAUDE.md, AGENTS.md…) say. |
| `verify` | After building: writes a tagged test plan onto the ticket, says where the change can be seen, hands the tests to whoever your agent files name, and records the results. |
| `ship` | After checking: works every open question left on the ticket, marks it Done once none is left and your team's definition of Done is met, then closes the chain (each parent is marked Done only once its own open questions are cleared). Safe to run more than once. |
| `retro` | Once an Initiative is Done (`ship` hands off when your setup says a retro is due), or whenever you ask: reads how its work went, lists earlier retro fixes still open or that did not hold, and proposes at most two changes, preferring a check, hook or tool rule to another instruction. Raises each approved change as a ticket. |
| `handover` | Only when you run it, before moving work to a new session: lists what this session decided or learned that is not on its tickets yet, writes it once you approve, then replies with a prompt of a few lines that starts the next session from the ticket. Writes no file. |

**Running a skill.** Each agent has its own way:

| Agent | Run a skill |
| --- | --- |
| Claude Code | `/madison:start-ticket ABC-12` (plugin skills always carry the plugin's name) |
| Cowork | `/start-ticket ABC-12` |
| Cursor | `/start-ticket ABC-12` in Agent chat |
| Codex | `$start-ticket ABC-12`, or `/skills` to pick one |
| Any other agent | Run it the way your agent runs skills |

In any agent you can also just ask ("start ticket ABC-12") and it picks the skill,
except `handover`, which never starts on its own: run it with its command. Each
skill ends by telling you the next step and the exact command for the agent that runs it.

## Before you start

Connect the Madison MCP server to your agent and sign in:

```bash
claude mcp add --transport http --scope user madison https://mcp.madison.works/mcp
```

Then run `/mcp` → `madison` → sign in. Other agents: add `https://mcp.madison.works/mcp`
as a remote MCP server named `madison`.

## Install

**Claude Code**

```text
/plugin marketplace add Madison-Inc/skills
/plugin install madison@madison
```

Or from a terminal: `claude plugin marketplace add Madison-Inc/skills`, then
`claude plugin install madison@madison`.

**Cowork**: on the Plugins page, click **+ Add** → **Add marketplace**, enter
`Madison-Inc/skills`, turn **Sync automatically** off and click **Sync**. Then, on the
**Discover** tab, click **Add** next to **Madison**. (Sync automatically needs the Claude
GitHub App on the repository, which only Madison can grant, so updates are checked by hand.)
The Claude desktop app's Code tab uses this same install.

**Other agents** ([skills CLI](https://github.com/vercel-labs/skills)):

```bash
npx skills add Madison-Inc/skills
```

A new workspace has no tickets yet, so start by describing the first problem you want to
solve with `create-problem`, which writes it up. There is no setup to fill in first: the
first time a skill needs to know how your team works (when a ticket is Done, which agent
does what), it asks that one question, recommends an answer, and saves it once you accept.

Updating a ticket needs no skill: ask your agent ("update ABC-12: …") and Madison's own
tools carry the rules.

## Updating

Each skill checks its version with Madison when it starts. When a newer one is out, it
says so and gives the full update route for your agent, then carries on. To update:

**Claude Code** (terminal or IDE)

```bash
claude plugin marketplace update madison
claude plugin update madison@madison
```

Then run `/reload-plugins`, or start a new session. Updating the marketplace alone does not
update the installed plugin. To update automatically instead, turn on auto-update for this
marketplace: `/plugin` → Marketplaces → madison → Enable auto-update. Claude Code leaves it
off for marketplaces other than its own.

**Cowork** (and the desktop app's Code tab): on the Plugins page, click **+ Add** → **Manage
marketplaces**, then **⋯** next to **skills** → **Check for updates**. Then quit the Claude app
(⌘Q on a Mac) and reopen it: the new version loads only after a restart.

**Other agents**:

```bash
npx skills update
```

## Status

Early. Only Claude Code is tested. The skills avoid Claude-only tools in their main steps,
so other agents should work; tell us if one doesn't.

Problems with a skill: open an issue on
[Madison-Inc/skills](https://github.com/Madison-Inc/skills/issues). This repository is
published from Madison's main repository, so pull requests here are not merged directly.

## Licence

MIT
