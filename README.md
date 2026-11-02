# Madison skills

Skills that let your agent work [Madison](https://madison.works) tickets the way a careful
teammate would: pick a ticket up, explore the code behind it, get a second opinion, build,
check and ship it your team's way, and write every decision and finding back onto the
ticket so the *why* outlives the session.

| Skill | When to use it |
| --- | --- |
| `setup-madison` | Once per workspace, and whenever how your team works changes. Records how your team works with Madison (when a ticket is Done, which agent does what, your ticket rules, …) so every other skill follows it. |
| `start-ticket` | Picking up a ticket: checks it is still relevant, explains it plainly, and works its open questions with you one at a time. |
| `explore` | Before building: reads the code against the ticket and posts what it found, plus the decisions still open, as a comment. |
| `review-comment` | When another agent has left a comment (an exploration, a review): explains it with a recommendation per decision, then posts your answers once you approve. |
| `build` | Once a ticket is decided: sets it In Progress and implements it, branching, reviewing and raising the change the way your own agent files (CLAUDE.md, AGENTS.md…) say. |
| `verify` | After building: writes a tagged test plan onto the ticket, says where the change can be seen, hands the tests to whoever your agent files name, and records the results. |
| `ship` | After checking: marks the ticket Done once your team's definition of Done is met, then closes the chain (parent statuses, leftover open questions). Safe to run more than once. |

**Running a skill.** Each agent has its own way:

| Agent | Run a skill |
| --- | --- |
| Claude Code | `/madison:start-ticket ABC-12` (plugin skills always carry the plugin's name) |
| Cowork | `/start-ticket ABC-12` |
| Cursor | `/start-ticket ABC-12` in Agent chat |
| Codex | `$start-ticket ABC-12`, or `/skills` to pick one |
| Any other agent | Run it the way your agent runs skills |

In any agent you can also just ask ("start ticket ABC-12") and it picks the skill. Each
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

**Cowork**: on the Plugins page, click **+ Add** → **Add marketplace**, enter
`Madison-Inc/skills`, turn **Sync automatically** off and click **Sync**. Then, on the
**Discover** tab, click **Add** next to **Madison**. (Sync automatically needs the Claude
GitHub App on the repository, which only Madison can grant, so updates are checked by hand.)
The Claude desktop app's Code tab uses this same install.

**Other agents** ([skills CLI](https://github.com/vercel-labs/skills)):

```bash
npx skills add Madison-Inc/skills
```

Then run `setup-madison` once. A new workspace has no tickets yet, so start by describing
the first problem you want to solve: setup-madison offers to write it up as a Problem, and
`start-ticket` takes it from there.

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
