---
name: explore
description: Explore the codebase against a Madison ticket before building - read the ticket, its comments and its parents, check the code, and post the findings and the decisions still open as a comment on the ticket. Use when the user asks to explore a ticket, or before implementing a Task or Bug. Does not write code.
---

This is Madison skills 0.1.5.

# Explore a ticket

Understand the work before anyone builds it. This skill never implements anything.

## Talk like this

- Short and plain. Lead with what matters.
- Check before you assert: every claim about the code names the file it came from.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## Steps

1. **Load the brief.**
   - `get_setup`: the team's rules and launch stage. They decide what counts as a risk;
     pre-live, there are no users to protect.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it in one line with your agent's update route from
     `version-check.md`, then carry on.
   - The ticket, with the matching `get_task`, `get_bug`, `get_initiative` or
     `get_problem` and `includeSuperseded: true`. A Task's response embeds its Initiative
     and Problems. A Task also returns `resolvedDesignLink`.
   - `list_comments` on the ticket and on each parent it embeds. The ticket wins over a
     comment; a contradiction becomes an open question.
   - **An earlier exploration on the thread is the starting point.** Re-check its claims,
     keep its decisions, and post only what changed or is still open.
   - **A design link** you can open: read it and explore against it. One you cannot open:
     stop and say so, naming the link. A missing link is fine.
2. **Read the code in a subagent.** Hand a fresh subagent the ticket's goal, what is
   already decided, and the questions to answer: which files and modules this touches,
   how it fits the existing patterns, edge cases, and anything in the code that conflicts
   with the ticket. Ask it to return conclusions with file paths, not file contents. If
   your agent has no subagents, do the reading yourself and keep notes short.
3. **Sort what came back.**
   - **Reality check:** what the code actually does today, where it differs from what the
     ticket assumes.
   - **Decisions you will take without asking:** the obvious calls, stated so they can be
     objected to.
   - **Open decisions:** numbered, each with the options and **a recommendation** with its
     reason. Rank by best outcome, never by effort. Leave out anything the ticket or its
     comments already settle.
4. **Post it as a comment** on the ticket with `create_comment`, before the user answers,
   so the analysis lives on the ticket. @mention the person who asked for the exploration,
   using a handle copied from `list_members` or `list_comments`, never one you made up.
5. **Show it in chat** in the same order, shorter.
6. **Hand off.** End your last message with one line naming the next step: `review-comment`
   on this comment, in the agent the team's `agentRoles` gives it to (only that agent's
   command; the Claude Code and Cowork forms both only when `agentRoles` is blank), with
   the exact command for that agent:
   Claude Code `/madison:review-comment <ticket>`; Cowork or Cursor `/review-comment <ticket>`; Codex
   `$review-comment <ticket>`; any other agent, name the skill in words. No advice about which
   session to use. Never end
   without this line.
