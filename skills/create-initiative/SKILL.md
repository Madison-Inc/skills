---
name: create-initiative
description: Write up a Madison Initiative - the direction chosen to solve a Problem - linked to that Problem, save it straight away, reply with its link, then work its open questions one at a time, including whether an MVP comes first. Use when the user wants to solve a Problem, commit to an approach, or asks to raise or write up an initiative.
---

This is Madison skills 0.3.0.

# Create an Initiative

An Initiative commits to a direction for solving one or more Problems. How to build it is
a Task's job, later.

## Talk like this

- Short and plain. Lead with the link.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **It links to a Problem.** No Problem yet: offer `create-problem` first and stop.
- **Search before you create**, with `search_records`.
- **Save first, no draft.** Never show a draft in chat or ask before saving. Save, reply
  with the link, then ask.
- **Always send `description`.**
- **Out of scope holds only a boundary someone drew.** Never invent an exclusion.
- **The MVP question is always asked:** could a perfectly built version still fail to
  solve the Problem? It is the first open question unless the user already answered it.

## Steps

1. **Load the brief.**
   - `get_setup` for the team's rules and launch stage.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - The Problem, with `get_problem`, and `list_comments` on it. If the user gave no
     Problem, find it with `search_records` and confirm it. A Done or Closed Problem
     takes no new Initiative: say so.
2. **Search.** `search_records` with the direction in the user's words. A strong match
   is probably this Initiative already: show it and offer to update or comment on it
   instead. Read `create-ticket.md` in this skill's folder for the score bands and how to
   pick the Area.
3. **Save it** with `create_initiative`, from the conversation and the Problem, nothing
   more:
   - `commitmentStatement`: the direction, not the implementation.
   - `linkedProblemId`: the Problem. `areaId`: the Problem's Area.
   - `description`: the why and the what.
   - `hypotheses`: why this direction will work. `inScope`, `outOfScope`, `constraints`
     and metrics only where the conversation gave them.
   - `decisions` already made, each with what it was chosen against.
   - `openQuestions`: the approach questions still open, with the MVP question first.
4. **Reply with the link** (`create-ticket.md` gives the format) and the commitment in one
   line.
5. **Work its open questions, one at a time.** Say how many there are, then ask only the
   first, with your recommendation, and wait for the answer. Record each answer on the
   Initiative with `update_initiative` before asking the next. Read `interview.md` in this
   skill's folder for how to word a recommendation and record an answer.
   - **The MVP answer.** If a perfect build could still fail, an MVP comes first: record
     "We decided to build an MVP first …" as a decision, the expectation in `hypotheses`,
     and say the MVP Task will carry the pass and stop bar in its success metric and
     acceptance criteria. If not, record that decision too, so `create-task` knows.
6. **Hand off.** End your last message with one line naming the next step: the first
   Task (the MVP, if one comes first) with `create-task`, in the agent the team's
   `agentRoles` gives it to (only that agent's command; the Claude Code and Cowork forms
   both only when `agentRoles` is blank): Claude Code `/madison:create-task <ticket>`;
   Cowork or Cursor `/create-task <ticket>`; Codex `$create-task <ticket>`; any other
   agent, name the skill in words. No advice about which session to use.
