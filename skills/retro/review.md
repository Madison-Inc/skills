# Retro review

Review how one Done Initiative's work went, from the process it left behind, and return
at most two proposed changes. Every piece of evidence names its source: a ticket id and
its comment, a decision quoted as it reads, or a session and its date. Anything you could
not read is listed as not read, never guessed.

## What you need

Fetch only what is listed here.

- The Initiative, with `get_initiative` and `includeSuperseded: true`, and
  `list_comments` on it.
- Its Tasks, with `list_tasks`, its `initiativeId` and `brief: true`: each Task's id,
  status and the decisions it overturned. Then `list_comments` on each Task. Never fetch
  a Task in full: its description, approach and acceptance criteria say what was built,
  not how the work went.
- **Not Done:** the Initiative is not Done, or any Task is not Done or Closed. Return
  just that, naming the open Tasks, and stop.
- Open retro tickets: `list_problems`, `list_initiatives`, `list_tasks` and `list_bugs`,
  each with `label: "retro"`. Each returns every status; drop Done and Closed yourself
  for the open list, and keep the Done Problems for the fixes check below.
- No git history, commits or pull requests.

## Past sessions

Read the sessions that mention the Initiative's ticket ids (the Initiative's and each
Task's), where your agent can. A session's corrections often never reached a ticket.

- **Claude Code:** session logs are JSONL files in the folders under
  `~/.claude/projects/` (one folder per working directory, worktrees included). Find the
  files that mention a ticket id (never as part of a longer id: ABC-12 must not match
  ABC-120). The most recently changed file is the session running this retro: read it
  too, but only what came before the retro was asked for. Read only a few hundred
  characters around each mention, never a whole file or line.
- **Cowork:** search past chats for each ticket id.
- **Any other agent:** sessions were not read.

If the logs are not where expected or cannot be searched, sessions were not read: say
so, and carry on with the tickets.

## What to look for

Where the work went wrong, each with its source:

- **Corrections:** the user telling an agent what it should have done, above all the
  same thing twice.
- **Rework:** an overturned decision, a Task Closed without being built, a review or test
  that sent work back, a question asked again after it was answered.
- **Fixes that did not hold:** a correction matching a Done Problem labelled `retro`
  (open it with `get_problem` when the title is close). Name the Problem.

## Proposals

Pick at most two changes, those that would have saved the most rework. For each, give:

- **What went wrong,** with its evidence.
- **The change** and its **kind:** a check, hook, tool rule, team rule, skill fix or
  agent-file change. A hook names the agent it needs.
- **Why not a check, hook or tool rule,** in one line, when the change adds an
  instruction (a team rule, a line in agent files or in a skill).
- **Where it lands:**
  - a team rule about ticket work: the team setup, and a ticket for the why;
  - how the team builds, branches, reviews or releases: its own agent files (CLAUDE.md,
    AGENTS.md…), through a ticket, never Madison's setup;
  - a check or hook in the team's own tools: a ticket;
  - a fault in a Madison skill or tool: an issue on the public skills repo. Draft its
    title and body naming only the skill, the step and what went wrong, in general words:
    no ticket ids, ticket text, names or code.

No change is worth making: say so. Two weak proposals are worse than none.

## Return

1. **Sources:** the tickets read, and which sessions were read (agent and dates) or that
   none were.
2. **Open retro tickets:** id, title and status of each, or "none".
3. **Fixes that did not hold:** each Done retro Problem a correction matched, with the
   correction, or "none".
4. **Proposals:** up to two, as above.
