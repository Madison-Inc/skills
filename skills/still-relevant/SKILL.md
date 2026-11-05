---
name: still-relevant
description: Re-check one open Madison ticket (Problem, Initiative, Task or Bug) against the code, other tickets and its parents' decisions, and propose Done, Closed or keep with the evidence, then record the verdict once the user approves. Use when the user asks whether a ticket is still relevant, still needed, already done, or can be closed.
---

This is Madison skills 0.5.1.

# Is this ticket still relevant?

Re-check one open ticket without picking it up. The check runs in a **fresh subagent**
given only the ticket id, so this session never holds the ticket in full. If your agent
has no subagents, do step 2's check yourself, from the ticket id alone.

## Talk like this

- Short and plain. Lead with the verdict.
- Every claim names its evidence: a commit, a file, a ticket id, or a decision.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **One ticket per run.** Never sweep a list of tickets.
- **The subagent gets only the ticket id, the team's `doneWhen` and step 2's
  instructions.** This session makes no `get_*` or `list_*` call for the ticket itself,
  except to look an answer up while working its open questions in step 4.
- **Write nothing until the user approves.** No comment and no status change before then.
- **Never set Done on a Task or Bug.** A Done verdict hands off to `verify` or `ship`.
- **An open question blocks Done.** A Done verdict on a Problem or Initiative that still
  has open questions works them first (step 4). Any still open: the verdict becomes keep.

## Steps

1. **Find the ticket.**
   - `get_setup`, passing `skillsVersion`: the version in the "This is Madison skills"
     line near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - **`doneWhen` blank:** the check needs it, and the subagent cannot ask. Ask it here
     first, recommend an answer and save it only once the user accepts, as
     `setup-question.md` in this skill's folder says. Declined: the subagent gets none.
   - The ticket id the user gave. Given only words, use `search_records`, show the best
     match and confirm it. No id and no words: ask for one.
2. **Check it in a subagent.** Give a fresh subagent the ticket id, `doneWhen` (or "none")
   and these instructions.
   It returns text, does not talk to the user and does not write to Madison.
   - Run the check in `still-relevant.md` in this skill's folder, and return what it says
     to return.
   - The ticket is already Done or Closed: return just that.
3. **Show the verdict** as it came back: the verdict, one line of why, the evidence, and
   what is unverified. A ticket already Done or Closed: say so and stop. Then ask one
   question:
   - **keep, marked *shipped, unlinked*:** was the shipped work this ticket's? Yes makes
     the verdict Done when every acceptance criterion is met, otherwise keep; no leaves it
     keep. It is never Closed.
   - **Any other verdict:** record it, or change it.
4. **Once approved,** in this session:
   - **Done on a Problem or Initiative with open questions:** first work them, one at a
     time, as `interview.md` in this skill's folder says. Clear each one: **answered** (a
     `decisions` or `findings` entry, then `removeOpenQuestions`), **moot** (removed with
     `removeOpenQuestions`, saying why), or **moved** to the open ticket (not Done or
     Closed) that will answer it (`openQuestions` there, then `removeOpenQuestions` here,
     its text exactly as on this ticket). Any still open: the verdict becomes keep,
     naming each question left.
   - **Every verdict:** one comment on the ticket (`create_comment`) giving the final
     verdict and the evidence.
   - **Closed:** set the ticket Closed with the matching `update_*` tool.
   - **Done on a Problem or Initiative:** set it Done with the matching `update_*` tool,
     once no open question is left on it.
   - **Done on a Task or Bug:** no status change.
   - **keep:** the comment only.
5. **Hand off.** End your last message with one line naming the next step, in the agent
   the team's `agentRoles` gives it to (only that agent's command; `agentRoles` blank: ask
   it first, recommend an answer and save it only once the user accepts, as
   `setup-question.md` in this skill's folder says; declined, give both the Claude Code
   and Cowork forms):
   - **Done on a Task or Bug:** `verify` when the ticket has no passing test results, or
     `ship` when it has.
   - **keep because step 4 left open questions on a Problem or Initiative:** `run-spike`
     on an Initiative when one needs evidence; otherwise `start-ticket` on the ticket.
   - **Any other keep:** `start-ticket` when the ticket is in Backlog; when it is already
     in progress, the step where it stands (`explore`, `build`, `verify` or `ship`).
   - **Closed, or Done on a Problem or Initiative:** say nothing is left on this ticket.

   Give the exact command for that agent: Claude Code `/madison:<skill> <ticket>`; Cowork
   or Cursor `/<skill> <ticket>`; Codex `$<skill> <ticket>`; any other agent, name the
   skill in words. No advice about which session to use.
