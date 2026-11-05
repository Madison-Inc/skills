---
name: handover
description: Before moving Madison work to a new session, write what this session decided or learned that is not on its tickets yet (once the user approves), then reply with a short prompt that starts the next session from the ticket. Run it only when the user asks for a handover; never start it on your own.
disable-model-invocation: true
---

This is Madison skills 0.5.1.

# Hand over to a new session

In Madison the ticket is the handover: every skill starts by reading the ticket, its
comments and its parents. What the ticket misses is what this session learned but never
wrote down. This skill writes that down, then gives a prompt of a few lines that points
the next session at the ticket.

## Talk like this

- Short and plain. Lead with what is missing from the tickets.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **Run only when the user asks.** Never offer a handover or a new session from here or
  from another skill, and say nothing about whether to open one.
- **Write nothing until the user approves,** and then only what they approved.
- **Never repeat what is already on a ticket.** Compare against its fields and comments;
  a reworded copy of an entry that is there already is a repeat.
- **Write no file.** No handoff note, no temp file, no change to the team's agent files
  (CLAUDE.md, AGENTS.md…). The ticket and the prompt are the whole output.
- **The prompt points at the ticket, never restates it.** It quotes none of the ticket's
  fields.

## Steps

1. **Load the brief.**
   - `get_setup`, passing `skillsVersion`: the version in the "This is Madison skills"
     line near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - **Every ticket this session worked on.** Use the ones the user names. Otherwise list
     the ticket ids this session read or wrote and confirm them with the user in one line.
     None at all: ask for one.
   - Each ticket with its `get_*` tool and `includeSuperseded: true`, and `list_comments`
     on each.
2. **Sweep the session.** Go through this conversation for anything that is not on those
   tickets yet. Compare against the tickets' fields and comments; do not re-read every
   tool result.
   - **A choice made** (by the user, or by you and not undone) is a decision: "We decided
     …", with `chosenAgainst` naming what it beat (nothing weighed: say why). When it
     overturns a decision already on the ticket, add `supersedes` with that statement
     copied exactly as the ticket has it. Name the active decision, never one already
     superseded.
   - **Something learned** that new evidence could make false is a finding: "We found …".
     A lesson about the repo (a test-fixture trap, a tool that misbehaves) is a finding
     like any other. Note it if it matters beyond this ticket: the prompt suggests it for
     the team's agent files.
   - **An open question this session answered:** the decision or finding that answers it,
     plus removing the question (`removeOpenQuestions`, its text exactly as on the
     ticket: a reworded one is silently ignored).
   - **Ticket admin** (status, priority, links, a follow-up filed) is never a decision: if
     it is missing, it goes in a comment on the ticket.
   - **Nothing new:** say so in one line, write nothing, and go to step 4.
3. **Show the list and wait.** Number every item and group them by ticket. Show each in
   full, exactly as it would be written: the ticket, the field, the text, and for a
   decision its `chosenAgainst` and any `supersedes`. Then stop until the user answers.
   - **Once approved,** write the approved items (with any changes the user made) in one
     `update_*` call per ticket (`decisions`, `findings`, `removeOpenQuestions`), and post
     any admin comment with `create_comment`. Write nothing they left out.
   - **The field check refuses an entry:** move it where the refusal says it belongs, or
     ask the user.
4. **Give the prompt.** End your reply with the prompt, in a code block of at most 6
   lines, and nothing after it.
   - **Line 1, the next step:** the hand-off line the last Madison skill in this session
     gave, when it still holds. Use `start-ticket` instead when no skill ran, that skill
     named no next step, or a later write changed what it was based on: on the ticket
     this session worked on last, or, when more than one was in play and it is not
     clear, the one the user names when you ask.
     For a create skill, the next step runs on the ticket its hand-off names (often the
     parent).
   - **The agent:** give the command only for the agent `agentRoles` gives that step to
     (`agentRoles` blank: ask it first, recommend an answer and save it only once the user
     accepts, as `setup-question.md` in this skill's folder says; declined, give both the
     Claude Code and Cowork forms). Claude Code `/madison:<skill> <ticket>`; Cowork or
     Cursor `/<skill> <ticket>`; Codex `$<skill> <ticket>`; any other agent, name the skill
     in words.
   - **Then only the traps** the next session needs that belong to neither the ticket
     nor the team's agent files, such as local state this session left (an unpushed
     branch, uncommitted changes), one line each. A repo lesson from step 2 that matters
     beyond the ticket gets one line suggesting the user add it to their agent files.
     More than fits in 6 lines: the extra traps belong on the ticket, so put them in
     step 2's list as findings rather than in the prompt.
   - Above the code block, link each ticket you wrote to, as a Markdown link with the
     ticket id as its text: `https://app.madison.works/dashboard/a/<area.key>/<type>/<ticketId>`
     when it has an Area, otherwise `https://app.madison.works/dashboard/<type>/<ticketId>`,
     where `<type>` is `problems`, `initiatives`, `tasks` or `bugs`.
