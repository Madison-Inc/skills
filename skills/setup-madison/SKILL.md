---
name: setup-madison
description: Record how your team works with Madison (when a ticket is Done, which agent does what, where designs live, when a retro runs, and your ticket rules), so every Madison skill follows it. Use when the user asks to set up Madison, to see or change how their team works with it, or to answer several setup questions at once. Never needed first. Each skill asks a blank answer when it needs it.
---

This is Madison skills 0.5.3.

# Set up Madison

Madison's ticket rules are fixed. How your team works with tickets is not: that lives in
the workspace's **team setup**, which every Madison skill reads with `get_setup` before it
starts. Each skill asks a blank answer the first time it needs it, so no team has to run
this first. This skill shows the setup, changes answers, and fills several at once.

How your team builds, branches, reviews, merges and releases is not part of it. That lives
in the team's own agent files (CLAUDE.md, AGENTS.md or whatever your agent reads), which
the build, verify and ship skills follow.

## Talk like this

- Short and plain. Lead with the answer.
- Check before you assert. Never state what the repo or a ticket says without reading it.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## Steps

1. **Read what is there.** Call `get_setup`. Answered fields are kept unless the user
   wants to change them; null fields are the ones to fill.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
2. **Work out what you can.** If you are in the team's code repository, read its agent
   files before asking anything: they often say when work counts as done or which agent
   does what. Each inference becomes a *proposed* answer, never a saved one.
3. **Ask the rest, one field at a time.** Ask **only one** field per message, with your
   proposed answer as the recommendation, and **wait for the answer** before the next.
   Skip fields already answered. Read `setup-question.md` in this skill's folder for each
   question, example answers and the fallback recommendation, and `interview.md` for how
   to word a recommendation.
4. **Save as you go.** Write each confirmed answer with `update_setup` straight away, and
   never one the user did not choose.
   **Team rules:** after the fields, ask "Is there anything your team always or never does
   with tickets that an agent should know?" Save each as its own rule with `addRules`, in
   the team's words (e.g. "Never reopen a ticket; a defect that comes back gets a new
   one"). Rules are for ticket work only: how the team branches, reviews, merges or
   releases, and facts about the code, belong in the repository's own agent files.
5. **Launch stage.** Show the launch stage from `get_setup` and ask whether it is right.
   Only the workspace owner can change it; if `update_setup` refuses, tell the user who can.
6. **Finish.** Show the saved setup in a short list. Then name the next step, which
   depends on whether the workspace has any tickets yet. Check rather than assume:
   `list_areas` gives each Area's `recordCount`, and a total of 0 means none.
   - **It has tickets:** `start-ticket` on the one the user wants to work first.
   - **It has none:** capture the first one with `create-problem`. Ask what problem they
     want to solve first, then run `create-problem` on their answer (or, in Madison's own
     chat, `/create-problem`).

   End with one line giving the exact command for the agent that runs it:
   Claude Code `/madison:<skill> <ticket>`; Cowork or Cursor `/<skill> <ticket>`; Codex
   `$<skill> <ticket>`; any other agent, name the skill in words. For `create-problem`
   there is no ticket yet: leave it off. No advice about which session to use.

If `update_setup` is refused because the user is not an owner or admin, say so, and stop
after showing what you would have saved.
