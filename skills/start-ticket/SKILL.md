---
name: start-ticket
description: Pick up an existing Madison ticket (Problem, Initiative, Task or Bug) - check it is still relevant, explain it in plain words, work its open questions one at a time, and set it In Progress. Use when the user says to start, pick up or tackle a ticket, or names a ticket to work on.
---

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
4. **Work the open questions.** If the ticket has any, follow `interview.md` in this
   skill's folder. Discovery questions belong on Problems, approach on Initiatives,
   implementation on Tasks and Bugs; if one is on the wrong ticket, say so.
5. **Set it In Progress** once the user is ready to work it, with the matching `update_*`
   tool. Do the same for its parents that are still in Backlog.
6. **Hand off.** Name the next step and say it belongs in a new session so it starts with
   a clean context: usually `explore` for a Task or Bug, or writing the next ticket down
   the chain for a Problem or Initiative.
