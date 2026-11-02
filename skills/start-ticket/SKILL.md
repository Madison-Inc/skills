---
name: start-ticket
description: Pick up an existing Madison ticket (Problem, Initiative, Task or Bug) - check it is still relevant, explain it in plain words, work its open questions one at a time, and set it In Progress. Use when the user says to start, pick up or tackle a ticket, or names a ticket to work on.
---

This is Madison skills 0.2.0.

# Start a ticket

## Talk like this

- Short and plain. Lead with the answer. No jargon without a plain gloss.
- Check before you assert. Say whether a claim comes from the ticket, a comment, or your
  own check of the code.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## Steps

1. **Load the context, and only what you need.**
   - `get_setup` for the team's rules and launch stage. If most answers are null, say so
     and suggest `setup-madison` after this ticket.
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
2. **Is it still relevant?** Check before explaining: is it already done in the code, or
   by another ticket (`search_records` with the ticket's own words)? Has a decision on a
   parent made it moot? If it is no longer relevant, say so with the evidence and stop.
3. **Explain it.** In a few sentences: what goes wrong, or what this changes, for the
   person using the product, with the ticket's own example if it has one. Then why, in a
   sentence or two. Then what is already decided.
4. **Work the open questions, one at a time.** These rules hold every time:
   - Say how many there are, then ask **only the first**, with your recommendation. One
     question per message.
   - **Wait for the user's answer.** Record it on the ticket before asking the next.
   - A question only the code can answer is not the user's: say so, leave it open for
     `explore`, and move to the next.

   Read `interview.md` in this skill's folder for how to word a recommendation and record
   an answer. Discovery questions belong on Problems, approach on Initiatives,
   implementation on Tasks and Bugs; if one is on the wrong ticket, say so.
5. **Set it In Progress** only once the questions the user can answer are answered (or the
   user says to stop asking), with the matching `update_*` tool. Do the same for its
   parents that are still in Backlog.
6. **Hand off.** End your last message with one line naming the next step: the skill, the
   agent the team's `agentRoles` gives it to (only that agent's command; the Claude Code
   and Cowork forms both only when `agentRoles` is blank), and the exact command for that
   agent: Claude Code `/madison:<skill> <ticket>`; Cowork or Cursor `/<skill> <ticket>`;
   Codex `$<skill> <ticket>`; any other agent, name the skill in words. No advice about
   which session to use. For a Task or Bug the next step is `build` when its decisions are
   settled, otherwise `explore`; for a Problem or Initiative it is writing the next ticket
   down the chain.
