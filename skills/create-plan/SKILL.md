---
name: create-plan
description: Start or grow a Madison Plan - an outcome to reach, the Problems and Bugs that serve it, what is not known yet and what is ruled out - save it straight away, reply with its link, then map the path one question at a time, raising each unknown as a Problem once it can be stated. Use when the user has an outcome or goal in mind and asks to plan towards it, map the path to it, or start or add to a Plan. Breaking an Initiative into Tasks is create-task, not this.
---

This is Madison skills 0.6.0.

# Create a Plan

A Plan is a path towards an outcome. It holds the Outcome, the Problems and Bugs that
serve it, Unknowns (things nobody can state precisely yet) and Out of scope (things ruled
out). It is not a ticket: it has no ticket id, and is addressed by its key. Decisions stay
on the linked tickets, never on the Plan.

## Talk like this

- Short and plain. Lead with the link.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **One Plan per outcome.** Look with `list_plans` before creating one (`search_records`
  does not see Plans). A matching Plan that is not Done or Closed is grown, not copied.
- **Save first, no draft.** Never show a draft in chat or ask before saving. The only
  question before saving is the outcome, and only when the user has not stated one.
- **A short title, the outcome in a few words**, in the user's words.
- **An Unknown is not a Problem until it can be stated.** Something nobody can describe
  precisely yet goes in Unknowns. Once it can be stated as a pain, raise it as a Problem,
  link it, and remove the Unknown.
- **A Problem raised here follows create-problem's rules:** search first, the pain and
  never the fix, always a `description`, discovery questions only.
- **Out of scope holds only what the user ruled out.** Never invent a boundary.
- **Only Problems and Bugs go in a Plan.** Never link an Initiative or a Task, and write
  no decisions onto the Plan.
- **One question per message**, each with a recommendation, and wait for the answer.

## Steps

1. **Load the setup.** `get_setup` for the team's rules and launch stage.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
2. **The outcome.** If the user has not said what is true once the Plan is reached, ask
   that one question first, with a recommended wording.
3. **Look for a Plan.** `list_plans`, and compare each title and outcome with this one.
   - **A match that is not Done or Closed:** offer to grow it. On a yes, `get_plan` and
     carry on with it from step 5. Create a second Plan only if the user says to.
   - **A match that is Done or Closed:** name it, say it cannot take more tickets, and
     start a new Plan.
4. **Save it** with `create_plan`: `title` and `outcome`. Reply with the link,
   `https://app.madison.works/dashboard/plans/<key>` as a Markdown link with the title
   as its text, in one line.
5. **Link what already exists.** `search_records` with the outcome in the user's words.
   Offer the Problems and Bugs that clearly serve it, in one question. Link each one the
   user accepts with `link_plan_ticket`; never recreate it. When a linked Problem already
   has an Initiative under it (`get_problem` shows its linked Initiatives), say in one
   line that the Plan counts that Problem as done only when the Problem itself is Done.
6. **Map the path.** Ask what else has to be true to reach the outcome, one question at a
   time, as `interview.md` in this skill's folder says. Write each answer to the Plan as
   it is given, with `update_plan`:
   - **Can be stated as a pain:** `search_records` first (`create-ticket.md` in this
     skill's folder gives the score bands). A strong match: link that ticket. Otherwise
     `create_problem` with `statement`, `description`, discovery `openQuestions` and
     `cwd`, then `link_plan_ticket`. If it was an Unknown, remove it (`removeUnknowns`,
     exact text).
   - **Cannot be stated yet:** add it to `unknowns`.
   - **Ruled out by the user:** add it to `outOfScope`.
   - **Needs evidence rather than a choice** (research, or throwaway code): leave it as an
     open question on its Problem. It is answered with `run-spike` once an Initiative is
     linked to that Problem.

   Stop when the user says the path is mapped, or every Unknown is either raised or still
   genuinely unclear.
7. **Hand off.** End your last message with one line naming the next step, for the
   Plan's first linked Problem that has no Initiative: `start-ticket` when it has open
   questions, `create-initiative` when it has none. Name the agent the team's
   `agentRoles` gives that skill to (only that agent's command; `agentRoles` blank: ask
   it first, recommend an answer and save it only once the user accepts, as
   `setup-question.md` in this skill's folder says; declined, give both the Claude Code
   and Cowork forms): Claude Code `/madison:<skill> <ticket>`; Cowork or Cursor
   `/<skill> <ticket>`; Codex `$<skill> <ticket>`; any other agent, name the skill in
   words. No such Problem: say every linked Problem already has an Initiative. No advice
   about which session to use.
