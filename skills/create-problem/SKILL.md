---
name: create-problem
description: Write up a Madison Problem - the pain a user or the business has, in their words - save it straight away, reply with its link, then work its open questions one at a time. Use when the user describes something going wrong, a pain point or a need, or asks to raise, capture or write up a problem.
---

This is Madison skills 0.5.1.

# Create a Problem

A Problem is the pain, never the fix. Solving it is an Initiative's job, later.

## Talk like this

- Short and plain. Lead with the link.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **The pain, never a solution or a tool.** The statement says what goes wrong and for
  whom, in words the user would say. "Add Sentry" is a fix; "We don't hear about errors
  until a user reports one" is the pain. A tool the user names as an example ("something
  like Sentry") goes in `openQuestions` as one option among others, never in the
  statement or description as the answer.
- **Search before you create**, with `search_records`.
- **Save first, no draft.** Never show a draft in chat or ask before saving. Save, reply
  with the link, then ask.
- **Always send `description`.**

## Steps

1. **Load the setup.** `get_setup` for the team's rules and launch stage.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
2. **Search.** `search_records` with the pain in the user's words. A strong match is
   probably this Problem already: show it and offer to update or comment on it instead.
   Read `create-ticket.md` in this skill's folder for the score bands.
3. **Save it** with `create_problem`, from what the user said and nothing more:
   - `statement`: one sentence, the pain.
   - `description`: the context behind it.
   - `severity` and `priority` from the bands on those fields.
   - `intents`, `userSegment`, `evidence` and `constraints` only where the user gave them.
   - `openQuestions`: what is still unknown about who is affected, how often, how badly
     and why it matters. Discovery questions only: how to build anything is not a
     Problem's question.
   - `cwd`, so it lands in the right Area.
4. **Reply with the link** (`create-ticket.md` gives the format) and the statement in one
   line.
5. **Work its open questions, one at a time.** Say how many there are, then ask only the
   first, with your recommendation, and wait for the answer. Record each answer on the
   Problem with `update_problem` before asking the next. Read `interview.md` in this
   skill's folder for how to word a recommendation and record an answer.
6. **Hand off.** End your last message with one line naming the next step: an Initiative
   to solve it, with `create-initiative`, in the agent the team's `agentRoles` gives it
   to (only that agent's command; `agentRoles` blank: ask it first, recommend an answer
   and save it only once the user accepts, as `setup-question.md` in this skill's folder
   says; declined, give both the Claude Code and Cowork forms): Claude Code
   `/madison:create-initiative <ticket>`; Cowork or
   Cursor `/create-initiative <ticket>`; Codex `$create-initiative <ticket>`; any other
   agent, name the skill in words. No advice about which session to use.
