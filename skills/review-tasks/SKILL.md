---
name: review-tasks
description: Get a second opinion on Madison Tasks before they are built - check each Task against its Initiative's decisions, whether its acceptance criteria can be tested, and the Task rules, with a recommendation per issue, then write the fixes and post the review once the user approves. Use after writing Tasks, or when the user asks to review, check or sanity-check a Task or an Initiative's Tasks.
---

This is Madison skills 0.3.3.

# Review Tasks

A second opinion on Tasks before anyone builds them. The review runs in a **fresh
subagent**, never in the session that wrote the Tasks, so it is not marking its own
homework. If your agent has no subagents and this session wrote or discussed the Tasks,
ask the user to run this skill in a new session, and stop. In a new session with no
subagents, do step 2's review yourself, from the ids alone.

## Talk like this

- Short and plain. Lead with the answer. No jargon without a plain gloss.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **The subagent gets only ticket ids and step 2's instructions.** Never your reasoning,
  a summary of the Tasks or why they were written that way: a reviewer handed the
  writer's case agrees with it.
- **Write nothing until the user approves.** No field change and no comment before then.
- **One question per message**, and wait for the answer before the next.

## Steps

1. **Find what to review.**
   - `get_setup`, passing `skillsVersion`: the version in the "This is Madison skills"
     line near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - The ticket id the user gave: an Initiative (review every open Task under it) or a
     Task (review that Task alone). Fetch nothing else here: the subagent finds the
     Tasks, so this session never holds them in full. No id: ask for one.
2. **Review in a subagent.** Give a fresh subagent the ticket id and these instructions.
   It returns the review as text, does not talk to the user and does not write to Madison.
   - **Find the Tasks.** An Initiative id: its open Tasks (`list_tasks` with its
     `initiativeId`; open means not Done or Closed). A Task id: that Task and its
     Initiative (none is itself an issue). None open, or the id is a Bug or Problem:
     return just that, and the session tells the user and stops.
   - Fetch the Initiative with `includeSuperseded: true`, `get_setup`, `get_task` for each
     Task, and `list_comments` on all of them. Read the team's agent files (CLAUDE.md,
     AGENTS.md…) for where a change can be seen; if there are none to read, say so and
     judge testability without them.
   - Check each Task for:
     1. **Decisions:** contradicting an active Initiative decision, or bringing back a
        superseded one. Name the decision.
     2. **Acceptance criteria:** each testable where a change can be seen, without
        anything the tester won't have (a second account, real user traffic, nothing
        measurable), and together covering the outcome. Give a testable rewording for
        any that fail. No test plan: that is `verify`'s.
     3. **Launch stage, only when it is pre-live:** flag caution for users who do not
        exist (migrations, flags, warnings, rollouts, back-compat kept for them). On a
        live workspace, skip this check.
     4. **Task rules:** a user story; a linked Initiative; no Bug's work; under an
        MVP-first Initiative, it is the MVP Task or a finding says the MVP passed.
     5. **Other Tasks:** two that contradict or duplicate each other. Name both.
   - Check the claims a recommendation rests on, in the code or in Madison; mark any it
     could not check as the Task's claim, unverified.
   - Return, per Task: id and title; **Verdict** (ready to build, or needs changes); then
     numbered **Issues**: what is wrong, **Recommend: X** with the field, the exact
     current text it replaces (quoted as stored) and the exact new text (for a field
     replaced whole, such as the description, the field's full new text), and one line of
     why. Rank by best outcome, never by
     effort. No issues: say so in one line.
3. **Show the review** as it came back. Then ask one question: apply every recommendation
   as it stands, or say which to change or skip. If the user wants to go through them,
   take one issue per message and wait for each answer.
4. **Once approved,** in this session:
   - Apply each accepted fix with `update_task`, using `remove*` or `replace*` to change
     an entry rather than appending a second one. A choice between options is a
     `decisions` entry ("We decided …", with `chosenAgainst`); one that overturns a
     decision on the Task names it in `supersedes`.
   - Post one comment on each Task reviewed (`create_comment`), covering that Task only:
     the verdict, each issue and what was done. A clash between two Tasks goes on both.
     Ticket admin (status, priority, splitting work) goes in the comment, not decisions.
5. **Hand off.** End your last message with one line naming the next step, in the agent
   the team's `agentRoles` gives it to (only that agent's command; the Claude Code and
   Cowork forms both only when `agentRoles` is blank), for the Task to build first:
   `start-ticket` when it has open questions, otherwise `explore`. Claude Code
   `/madison:<skill> <ticket>`; Cowork or Cursor `/<skill> <ticket>`; Codex
   `$<skill> <ticket>`; any other agent, name the skill in words. No advice about which
   session to use.
