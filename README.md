# Madison skills

Skills that let your agent work [Madison](https://madison.works) tickets the way a careful
teammate would: pick a ticket up, explore the code behind it, get a second opinion, and
write every decision and finding back onto the ticket so the *why* outlives the session.

| Skill | When to use it |
| --- | --- |
| `setup-madison` | Once per workspace, and whenever how your team works changes. Records your team's answers (branching, who merges, when a ticket is Done, …) in Madison so every other skill follows them. |
| `start-ticket` | Picking up a ticket: checks it is still relevant, explains it plainly, and works its open questions with you one at a time. |
| `explore` | Before building: reads the code against the ticket and posts what it found, plus the decisions still open, as a comment. |
| `review-comment` | When another agent has left a comment (an exploration, a review): explains it with a recommendation per decision, then posts your answers once you approve. |

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

Then run `setup-madison` once.

## Status

Early. Only Claude Code is tested. The skills avoid Claude-only tools in their main steps,
so other agents should work; tell us if one doesn't.

Problems with a skill: open an issue on
[Madison-Inc/skills](https://github.com/Madison-Inc/skills/issues). This repository is
published from Madison's main repository, so pull requests here are not merged directly.

## Licence

MIT
