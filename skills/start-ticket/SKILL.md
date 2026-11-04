---
name: start-ticket
description: Pick up an existing Madison ticket (Problem, Initiative, Task or Bug) - check it is still relevant, explain it in plain words, work its open questions one at a time, and set it In Progress. Use when the user says to start, pick up or tackle a ticket, or names a ticket to work on.
---

This is Madison skills 0.4.0.

# Start a ticket

## Talk like this

- Short and plain. Lead with the answer. No jargon without a plain gloss.
- Check before you assert. Say whether a claim comes from the ticket, a comment, or your
  own check of the code.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## Steps

1. **Load the context, and only what you need.**
   - `get_setup` for the team's rules and launch stage. A blank answer is asked only when
     a step needs it, as `setup-question.md` in this skill's folder says.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - Find the ticket. Given an id, fetch it with the matching `get_problem`,
     `get_initiative`, `get_task` or `get_bug` and `includeSuperseded: true`. If the type
     is wrong, try the others. Given only words, use `search_records`, show the best match
     and confirm it. Never page through `list_*` tools to find a ticket.
   - `list_comments` on the ticket, and on each parent the response embeds (a Task's
     Initiative and its Problems; an Initiative's Problems). Comments are part of the brief:
     they often hold answers, scope calls and earlier explorations. Where a comment
     contradicts the ticket, the ticket wins; raise it as a question.
2. **Is it still relevant?** Check before explaining, as `still-relevant.md` in this
   skill's folder says (it reads `doneWhen`: blank, ask it first, recommend an answer and
   save it only once the user accepts, as `setup-question.md` says): is it already done in
   the code, or by another ticket? Has a
   decision on a parent made it moot? **If the verdict is Done or Closed, or keep marked
   *shipped, unlinked*, say so with the evidence and stop:** start no questions, write no
   status, and name `still-relevant` as the next step (step 6's hand-off line), which
   records the verdict. On any other keep, carry on. **A Done verdict on a ticket that
   still has open questions is not Done:** an open question blocks Done, so say so and
   carry on to work them.
3. **Explain it.** In a few sentences: what goes wrong, or what this changes, for the
   person using the product, with the ticket's own example if it has one. Then why, in a
   sentence or two. Then what is already decided.
4. **Work the open questions, one at a time.** These rules hold every time:
   - Say how many there are, then ask **only the first**, with your recommendation. One
     question per message.
   - **Wait for the user's answer.** Record it on the ticket before asking the next.
   - A question only the code can answer is not the user's: say so, leave it open for
     `explore`, and move to the next.
   - A question on an Initiative or Task that needs evidence the code cannot give in a
     minute (research, or throwaway code to try something) is not the user's either:
     say so in one line, leave it open for `run-spike`, and move to the next.

   Read `interview.md` in this skill's folder for how to word a recommendation and record
   an answer. Discovery questions belong on Problems, approach on Initiatives,
   implementation on Tasks and Bugs; if one is on the wrong ticket, say so.
5. **Set it In Progress** only once the questions the user can answer are answered (or the
   user says to stop asking), with the matching `update_*` tool. Do the same for its
   parents that are still in Backlog.
6. **Hand off.** End your last message with one line naming the next step: the skill, the
   agent the team's `agentRoles` gives it to (only that agent's command; `agentRoles`
   blank: ask it first, recommend an answer and save it only once the user accepts, as
   `setup-question.md` in this skill's folder says; declined, give both the Claude Code
   and Cowork forms), and the exact command for that
   agent: Claude Code `/madison:<skill> <ticket>`; Cowork or Cursor `/<skill> <ticket>`;
   Codex `$<skill> <ticket>`; any other agent, name the skill in words. No advice about
   which session to use. When step 2 stopped, the next step is `still-relevant`. When a question was left for
   `run-spike`, the next step is `run-spike` on this ticket. When step 2 carried on past a
   Done verdict, the next step is `ship` for a Task or Bug, or `still-relevant` for a
   Problem or Initiative. Otherwise,
   for a Task or Bug the next step is `build` when its decisions are
   settled, otherwise `explore`; for a Problem it is `create-initiative` when it has no
   open Initiative, otherwise `start-ticket` on that Initiative; for an
   Initiative it is `create-task`, or `create-design` first when `designTool` is set and
   not "none yet" (blank counts as none: do not ask it), the Initiative has no
   `designLink`, and its `inScope` changes a screen or anything else users see.
