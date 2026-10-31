# Madison skills

Skills that let your agent work [Madison](https://madison.works) tickets the way a careful
teammate would: pick a ticket up, explore the code behind it, get a second opinion, build,
check and ship it your team's way, and write every decision and finding back onto the
ticket so the *why* outlives the session.

| Skill | When to use it |
| --- | --- |
| `setup-madison` | Once per workspace, and whenever how your team works changes. Records your team's answers (branching, who merges, when a ticket is Done, …) in Madison so every other skill follows them. |
| `start-ticket` | Picking up a ticket: checks it is still relevant, explains it plainly, and works its open questions with you one at a time. |
| `explore` | Before building: reads the code against the ticket and posts what it found, plus the decisions still open, as a comment. |
| `review-comment` | When another agent has left a comment (an exploration, a review): explains it with a recommendation per decision, then posts your answers once you approve. |
| `build` | Once a ticket is decided: sets it In Progress and implements it, branching, reviewing and raising the change the way your setup says. |
| `verify` | After building: writes a tagged test plan onto the ticket, says where the change can be seen, hands the tests to whoever your setup names, and records the results. |
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

**Cowork**: download `madison-<version>.plugin` from the
[latest release](https://github.com/Madison-Inc/skills/releases/latest) and upload it in
Cowork's plugin settings.

**Other agents** ([skills CLI](https://github.com/vercel-labs/skills)):

```bash
npx skills add Madison-Inc/skills
```

Then run `setup-madison` once. A new workspace has no tickets yet, so start by describing
the first problem you want to solve: setup-madison offers to write it up as a Problem, and
`start-ticket` takes it from there.

## Updating

Each skill checks its version with Madison when it starts, and says so in one line when a
newer one is out. To update:

**Claude Code**

```bash
claude plugin marketplace update madison
claude plugin update madison@madison
```

Then run `/reload-plugins`, or start a new session. Updating the marketplace alone does not
update the installed plugin. To update automatically instead, turn on auto-update for this
marketplace: `/plugin` → Marketplaces → madison → Enable auto-update. Claude Code leaves it
off for marketplaces other than its own.

**Cowork**: download the new `madison-<version>.plugin` from the
[latest release](https://github.com/Madison-Inc/skills/releases/latest) and upload it in
Cowork's plugin settings.

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
