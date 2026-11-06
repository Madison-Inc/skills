---
name: create-task
description: Write up a Madison Task - one unit of work that implements an Initiative - linked to that Initiative with a user story, save it straight away and reply with its link. Under an Initiative building an MVP first, it writes only the MVP Task until that passes. Use when the user wants to plan, break down or write up work for an Initiative.
---

This is Madison skills 0.5.3.

# Create a Task

A Task is the unit of work that implements an Initiative, whoever it is for. A defect is
a Bug, not a Task: use `create-bug`.

## Talk like this

- Short and plain. Lead with the link.
- Check before you assert: every claim about the code names the file it came from.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **It links to an Initiative**, with `linkedInitiativeId`. No Initiative yet: offer
  `create-initiative` first and stop. Never create a Task without one.
- **It has a user story**, "As a …, I want …, so that …", even for engineering work,
  where the person may be a maintainer or an agent.
- **MVP first means MVP only.** If the Initiative decided to build an MVP first, write
  only the MVP Task. Every other Task waits until the MVP has passed: decline them, say
  why, and say they come after the MVP passes `verify`.
- **Never a Task to do a Bug's work.**
- **Search before you create**, with `search_records`.
- **Save first, no draft.** Never show a draft in chat or ask before saving.
- **Always send `description`.**

## Steps

1. **Load the brief.**
   - `get_setup` for the team's rules and launch stage.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - The Initiative, with `get_initiative` and `includeSuperseded: true`, and
     `list_comments` on it. If the user gave none, find it with `search_records` and
     confirm it. A Done or Closed Initiative takes no new Task: say so.
2. **Check the MVP gate.** The Initiative is MVP-first when an active decision says it
   builds an MVP first. Then `list_tasks` with its `initiativeId`:
   - **No MVP Task yet:** the Task you write is the MVP. Its `successMetric` and
     `acceptanceCriteria` carry the pass and stop bar from the Initiative.
   - **An MVP Task exists and the Initiative has no finding that it passed:** write
     nothing. Say the next Tasks wait for the MVP, name it, and stop.
   - **It passed** (a finding on the Initiative says so): carry on as normal.
3. **Search.** `search_records` with the work in the user's words. A strong match is
   probably this Task already: show it and offer to update or comment on it instead.
   Read `create-ticket.md` in this skill's folder for the score bands and how to pick the
   Area.
4. **Look at the code** if you are in the repository, enough to name the files and the
   approach. Keep it short; deeper reading is `explore`'s job.
5. **Save it** with `create_task`, one Task per call:
   - `title`: what gets built or changed. `linkedInitiativeId`, and `areaId` from the
     Initiative.
   - `userStory`, `description` and `outcome`.
   - `approach`, `impactAreas` (real files and modules), `acceptanceCriteria` (testable),
     `risks` and `dependencies` where known.
   - `decisions` already made, and `openQuestions`: implementation questions only.
   - No `testingSteps`: the test plan is `verify`'s.
6. **Reply with the link** (`create-ticket.md` gives the format) and the title in one line.
   If you declined any Tasks under the MVP rule, say which.
7. **Hand off.** End your last message with one line naming the next step, in the agent
   the team's `agentRoles` gives it to (only that agent's command; `agentRoles` blank: ask
   it first, recommend an answer and save it only once the user accepts, as
   `setup-question.md` in this skill's folder says; declined, give both the Claude Code
   and Cowork forms): `review-tasks`, for a second
   opinion before anything else (pass the Initiative id when you wrote several Tasks, so
   clashes between them are caught; otherwise the Task id). After the review comes
   `start-ticket` when it has open questions, otherwise `explore`: say so in the same
   line. Claude Code `/madison:<skill> <ticket>`; Cowork or
   Cursor `/<skill> <ticket>`; Codex `$<skill> <ticket>`; any other agent, name the skill
   in words. No advice about which session to use.
