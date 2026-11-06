---
name: retro
description: Look back on a Done Madison Initiative - how its work went, which earlier retro fixes are still open or did not hold - and propose at most two changes, preferring a check, hook or tool rule over another instruction, then raise each approved change as a ticket. Use when an Initiative is Done and the team's setup says a retro is due, or when the user asks for a retro or what to learn from an Initiative.
---

This is Madison skills 0.5.3.

# Retro

Look back on how one Done Initiative's work went, and change how the team works only
where the evidence says it went wrong. The reading runs in a **fresh subagent** given only
the Initiative id, so this session never holds the Initiative's history. If your agent
has no subagents, do step 2's reading yourself, from the Initiative id alone.

## Talk like this

- Short and plain. Lead with what went wrong, if anything.
- Every claim names its evidence: a ticket id and comment, a decision, or a session.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **One Initiative per run,** and only once it is Done.
- **The subagent gets only the Initiative id and step 2's instructions.** This session
  makes no `get_*` or `list_*` call for the Initiative, its Tasks or their comments.
- **Never read a Task's full body.** The Tasks are read as ids, statuses and overturned
  decisions (`list_tasks` with `brief: true`), plus their comments: how the work went,
  not what was built.
- **At most two changes.** Prefer a check, hook or tool rule to another instruction: a
  change that adds an instruction says in one line why none of those would do.
- **Write nothing until the user approves:** no ticket, comment, setup change or issue.
- **Nothing private goes into a public issue.** An issue for Madison's own skills or
  tools names only the skill, the step and what went wrong, in general words: no ticket
  ids, ticket text, names or code.
- **Read no git history,** and assume nothing about how the team builds, branches or
  releases.

## Steps

1. **Find the Initiative.**
   - `get_setup`, passing `skillsVersion`: the version in the "This is Madison skills"
     line near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - The Initiative id the user or `ship` gave. Given only words, use `search_records`,
     show the best match and confirm it. No id and no words: ask for one.
2. **Read it in a subagent.** Give a fresh subagent the Initiative id and these
   instructions. It returns text, does not talk to the user and does not write to Madison.
   - Run the review in `review.md` in this skill's folder, and return what it says to
     return.
3. **Show the review** as it came back: the sources read and not read, open retro
   tickets, fixes that did not hold, and the proposals. Not Done: say which Tasks are open
   and stop. Then take each proposal in its own message: approve, change or drop. Wait
   for each answer.
4. **Once approved,** in this session, for each approved change:
   - **A change the team makes** (a team rule, a check, a hook, a line in its agent
     files): `search_records` with the change's words, reading the scores as
     `create-ticket.md` says.
     - **An open Problem matches:** add the evidence with `update_problem`. If one of its
       linked Initiatives (from `get_problem`) is in the review's open retro tickets,
       comment on that Initiative instead of raising another.
     - **No match, or the match is Done or Closed:** `create_problem` labelled `retro`,
       its description naming any Done or Closed match ("raised again: the fix in ABC-12
       did not hold").
     - Then `create_initiative` labelled `retro`, linked to that Problem with
       `linkedProblemId`, its description opening "Raised by a retro on <Initiative id>."
     - **A team rule** is also written straight away with `update_setup` (`addRules`).
       Refused for lack of role: say an owner or admin can add it, and note "not yet in
       setup" on the new Initiative.
   - **A fault in Madison's own skills or tools:** no ticket in the team's backlog. Show
     the issue's exact title and body, then a new-issue link,
     `https://github.com/Madison-Inc/skills/issues/new?title=<title>&body=<body>`, both
     URL-encoded. An agent with the GitHub CLI may raise it instead, only once the user
     approves that exact text.
   - Then one comment on the reviewed Initiative (`create_comment`): that a retro ran,
     the open retro tickets and fixes that did not hold, and what was raised, with links.
     Nothing approved: say so in the comment.
5. **Hand off.** End your last message with one line naming the next step, in the agent
   the team's `agentRoles` gives it to (only that agent's command; `agentRoles` blank: ask
   it first, recommend an answer and save it only once the user accepts, as
   `setup-question.md` in this skill's folder says; declined, give both the Claude Code
   and Cowork forms): `start-ticket` on the first
   Initiative raised, or, when nothing was raised, that nothing is left. Claude Code
   `/madison:<skill> <ticket>`; Cowork or Cursor `/<skill> <ticket>`; Codex
   `$<skill> <ticket>`; any other agent, name the skill in words. No advice about which
   session to use.
