---
name: create-design
description: Make the design for a Madison Initiative in your team's design tool, from what the ticket says, and set the Initiative's design link so every Task under it is handed the design. Revises the existing design in place when there is one. Use when an Initiative changes what users see and has no design yet, when the user asks for a design, mock-up or wireframe for an Initiative, or when a hand-off names it.
---

This is Madison skills 0.4.0.

# Create a design

The design for a piece of work is agreed on its Initiative. Its Tasks inherit the link,
and `explore` and `build` read it from there. This skill makes that design and sets the
link. It writes no code.

## Talk like this

- Short and plain. Lead with the design link.
- Check before you assert: every screen in the design traces to something on the ticket.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **Read `designTool` before anything else.** A design is made only in the team's own
  tool. Never fall back to another tool, an HTML page, an image or a local file: build
  would be handed a link that is not the team's design.
- **This agent can't reach the tool: stop.** Create nothing, set no link, say so in one
  line, and hand off.
- **Set the link only once the design exists and opens.** Never set a link you have not
  opened.
- **Never replace an existing design link silently.** Offer to revise that design in
  place, keeping its link. Make a new one only when the user asks.
- **Design from the ticket, not from guesses.** What it does not settle, leave out and
  name as a gap.

## Steps

1. **Load the brief.**
   - `get_setup` for the team's answers and launch stage.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - The Initiative, with `get_initiative` and `includeSuperseded: true`, its Problems
     (embedded), and `list_comments` on it. Given a Task, use `get_task` instead: its own
     design differs from its Initiative's, so its link goes on the Task. A Done or Closed
     ticket takes no new design: say so and stop.
2. **Check the design tool.** Read `designTool`.
   - **Blank:** ask it as `setup-question.md` in this skill's folder says, then carry on.
   - **"None yet", or declined:** say in one line there is nowhere to make a design, and
     stop. Create nothing and set no link.
   - **Claude Design:** read `tools.md` in this skill's folder. If this agent has no
     Design type, say so in one line, create nothing, and hand off (step 7).
   - **Any other tool:** say in one line that this skill can't make a design there yet.
     Ask the user to make it (or change the existing one) and give you its https link.
     If you can open it, check it opens; if you have no tool for that site, ask the user
     to confirm it opens. Then go to step 3 with it, never straight to step 5.
3. **An existing design link** on the ticket: show it and offer to revise that design.
   Revise it in place unless the user asks for a new one. A link that does not open:
   say so, and ask whether to make a new design. Only the user's explicit "replace it"
   lets a new link take the old one's place.
4. **Make the design** in the team's tool, the way `tools.md` says (skip this for any
   other tool: the user made it). Brief it from the ticket only. For an Initiative: its
   statement and description, `inScope` items that change what users see, active
   decisions, constraints and user segment, and the Problem it solves; leave out
   `outOfScope` items and anything a superseded decision ruled out. For a Task: its user
   story, description and acceptance criteria, plus its Initiative's fields as above.
   The design system is the tool's to pick. Then read the design back against the
   ticket: every item users see is shown, and nothing contradicts a decision or an
   out-of-scope item. Fix any gap before going on.
5. **Set the link** with `update_initiative` (`designLink`), or `update_task` for a Task.
   Skip this when you revised the design in place and its link is unchanged.
6. **Reply** with the design link, then the ticket link (`create-ticket.md` in this
   skill's folder gives the format), and one line on anything the ticket left unsettled
   that the design had to leave out.
7. **Hand off.** End your last message with one line naming the next step, in the agent
   the team's `agentRoles` gives it to (only that agent's command; `agentRoles` blank: ask
   it first, recommend an answer and save it only once the user accepts, as
   `setup-question.md` in this skill's folder says; declined, give both the Claude Code
   and Cowork forms):
   - **Stopped because this agent has no Design type:** `create-design` on this ticket,
     in the agent that does design work. `agentRoles` set but naming no agent for
     design: give both the Claude Code and Cowork forms, without asking.
   - **An Initiative:** `list_tasks` with its `initiativeId` and `brief: true`. No open
     Tasks: `create-task`. Open Tasks: `explore` on each, so they are checked against
     the design.
   - **A Task:** `explore` on it.
   Claude Code `/madison:<skill> <ticket>`; Cowork or Cursor `/<skill> <ticket>`; Codex
   `$<skill> <ticket>`; any other agent, name the skill in words. No advice about which
   session to use.
