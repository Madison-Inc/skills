---
name: create-bug
description: Write up a Madison Bug - something that is broken, with how to reproduce it - save it straight away and reply with its link. Use when the user reports a defect, a regression or something not working as it should, or asks to raise, file or log a bug.
---

This is Madison skills 0.4.1.

# Create a Bug

A Bug is a defect, and it is its own implementation ticket: the fix is worked on the Bug.

## Talk like this

- Short and plain. Lead with the link.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **The title says what is broken, never the fix.** "Export button does nothing on
  Safari", never "Fix the export handler".
- **The fix is worked on the Bug itself.** Never create a Task to do a Bug's work.
- **A Bug never links to an Initiative.** Send no parent, even when the user names one.
- **Search before you create**, with `search_records`.
- **Save first, no draft.** Never show a draft in chat or ask before saving.
- **Always send `description`.**

## Steps

1. **Load the setup.** `get_setup` for the team's rules and launch stage.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
2. **Search.** `search_records` with what is broken, in the user's words. A strong match
   is probably this Bug already: show it and offer to comment on it instead. If the match
   is Done or Closed, the defect has come back: that is a new Bug, so carry on and mention
   the old one in the description. Read `create-ticket.md` in this skill's folder for the
   score bands.
3. **Save it** with `create_bug`, from what the user said and what you can see in the
   code, nothing more:
   - `title`: what is broken.
   - `description`: the context behind it.
   - `stepsToReproduce`, `expectedBehavior` and `actualBehavior`, kept distinct. A step
     the user did not give is an open question, never a guess.
   - `severity` and `priority` from the bands on those fields.
   - `openQuestions`: what is still unknown about reproducing it or fixing it.
   - `cwd`, so it lands in the right Area.
4. **Reply with the link** (`create-ticket.md` gives the format) and the title in one
   line. If it has open questions, list them.
5. **Hand off.** End your last message with one line naming the next step, in the agent
   the team's `agentRoles` gives it to (only that agent's command; `agentRoles` blank: ask
   it first, recommend an answer and save it only once the user accepts, as
   `setup-question.md` in this skill's folder says; declined, give both the Claude Code
   and Cowork forms): `start-ticket` when it has open
   questions, otherwise `explore`. Claude Code `/madison:<skill> <ticket>`; Cowork or
   Cursor `/<skill> <ticket>`; Codex `$<skill> <ticket>`; any other agent, name the skill
   in words. No advice about which session to use.
