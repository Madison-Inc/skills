---
name: setup-madison
description: Record how your team works (branch naming, who merges, when a ticket is Done, where changes can be seen, which agent does what) in Madison, so every Madison skill follows it. Use once per workspace, when the user asks to set up Madison, or when another Madison skill finds the setup unanswered.
---

# Set up Madison

Madison's ticket rules are fixed. How your team ships is not: that lives in the workspace's
**team setup**, which every Madison skill reads with `get_setup` before it starts. This
skill fills it in.

## Talk like this

- Short and plain. Lead with the answer.
- Check before you assert. Never state what the repo or a ticket says without reading it.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## Steps

1. **Read what is there.** Call `get_setup`. Answered fields are kept unless the user
   wants to change them; null fields are the ones to fill.
2. **Work out what you can.** If you are in the team's code repository, read it before
   asking anything: the default branch and recent branch names (`git branch -r`), branch
   protection if a GitHub CLI is available, CI and deploy config (preview deploys, which
   branch deploys), and how tests run. Each inference becomes a *proposed* answer, never
   a saved one.
3. **Ask the rest, one field at a time.** Ask **only one** field per message, with your
   proposed answer as the recommendation, and **wait for the answer** before the next.
   Skip fields already answered. Read `questions.md` in this skill's folder for what each
   field means and example answers, and `interview.md` for how to word a recommendation.
4. **Save as you go.** Write each confirmed answer with `update_setup` straight away. Team
   rules (anything the team always or never does) go in `addRules`, one rule per item.
5. **Launch stage.** Show the launch stage from `get_setup` and ask whether it is right.
   Only the workspace owner can change it; if `update_setup` refuses, tell the user who can.
6. **Finish.** Show the saved setup in a short list. Then name the next step, which
   depends on whether the workspace has any tickets yet. Check rather than assume:
   `list_areas` gives each Area's `recordCount`, and a total of 0 means none.
   - **It has tickets:** `start-ticket` on the one the user wants to work first.
   - **It has none:** capture the first one. Ask what problem they want to solve first, and
     write it up as a Problem with `create_problem` (or in Madison's own chat, with
     `/problem`). Picking it up with `start-ticket` comes after that.

   End with one line giving the exact command for the agent that runs it:
   Claude Code `/madison:start-ticket <ticket>`; Cowork or Cursor `/start-ticket <ticket>`; Codex
   `$start-ticket <ticket>`; any other agent, name the skill in words.
   No advice about which session to use.

If `update_setup` is refused because the user is not an owner or admin, say so, and stop
after showing what you would have saved.
