---
name: onboard
description: "Tell Madison what your product is and who it is for: signs you in to Madison, reads your code and what your agent already knows, asks you only what it cannot work out, shows the notes it will save and saves them as your workspace's Context once you agree. Use when the user runs onboarding for a new Madison workspace, or asks to set up or refresh what Madison knows about their product."
---

This is Madison skills 0.9.0.

# Onboard a workspace

Madison loads a short set of notes, the **Workspace Context**, into every chat in the
workspace. It holds only what she cannot read from the code when she needs it. Your job is
to write the first set from what you can already see, ask the user only what you cannot
work out, and save it once they agree.

## Talk like this

- Short and plain. The user may not be technical: no jargon, no file paths in questions.
- Check before you assert: every note comes from something you read or the user said.
- Ask in plain prose, one question at a time, never with a multiple-choice question
  dialog or form.

## What a note may hold, and what it never holds

Every note is one short line: one fact or one rule.

- **Belongs:** what the product is; who it is for; rules and constraints that leave no
  trace in the code (security or compliance rules, data that must never be stored, a
  feature built but deliberately switched off, things the team has ruled out).
- **Never:** architecture, stack, libraries, file layout, install or run steps, how to
  contribute, current priorities, what is being worked on now, or anything else Madison
  can read from the code or the tickets when she needs it. If a note would go stale the
  next time the code changes, leave it out.
- **No guesses.** A note you have no evidence for becomes a question, or is left out.

## Steps

1. **Sign in.** Call `get_setup`.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - **If Madison's tools are missing, or a call says Madison needs authentication,** in
     Claude Code tell the user exactly this: "Type `/mcp`, select **madison**, choose
     **Authenticate**, then click **Allow** in the browser window that opens. Tell me when
     it's done." In any other agent, tell them to sign in to the Madison MCP server the way
     that agent signs in to one. **Never call an `authenticate` tool or show a sign-in
     link yourself:** that path fails. When they say it is done, call `get_setup` again,
     and go on only once it works.
   - If the call asks which workspace to use, call `list_workspaces`, ask the user which
     one they are setting up, and call `use_workspace` with their answer.
2. **Read what Madison already has.** Call `get_context`. Notes already there are kept
   unless they are wrong or break the rules above: you correct one by its id (`replace`)
   or `remove` it, never add a second note that says the same thing.
3. **Work out what you can.** Before asking anything, read:
   - the repository: its README and docs, the agent files (CLAUDE.md, AGENTS.md and the
     like), the product's own words (landing page, marketing copy, terms, privacy policy,
     onboarding screens), and settings or flags that switch something off on purpose;
   - what you already know about this product from your own memory and from earlier
     sessions with this user, where your agent keeps them.

   Take from these only what the rules above allow.
4. **Draft the notes.** Usually three to ten. Keep them under the character cap
   `get_context` reports.
5. **Ask only what you could not work out.** Every question must be about the product's
   purpose, its users, a rule the code does not show, or whether it is live. Never ask for
   a file, a document, a technical detail, or anything the repository already says.
   - **One question per message,** with your best guess as a suggested answer, and wait
     for the reply before the next.
   - At most five questions. If the user does not know, leave that note out.
6. **Launch stage.** Look at `launchStageConfirmed` in the `get_setup` answer from step 1.
   **If it is true, do not ask:** someone has already chosen the stage. If it is false, ask
   whether the product is live to real users or still being built, suggesting an answer
   from what you saw in the code (for example, sign-up and payments switched on, or a
   production site, suggests live).
7. **Show everything before saving.** Show the full set as a numbered list: each new
   note, each correction (old text, then new) and each removal, plus the launch stage if
   you asked it. Ask whether to save it. Change anything the user asks and show the whole
   set again. **Save nothing until the user has seen the full set and agreed.**
8. **Save.** Send every note change in **one** `update_context` call. If it comes back
   with `updated: false`, read its `message`, fix the call, and if the notes change, show
   them again before retrying. Then save the launch stage with `update_setup`
   (`launchStage`: `LIVE` or `PRE_LIVE`), if you asked it. If `update_setup` refuses it,
   the notes are still saved: tell the user only the workspace owner can set whether the
   product is live, and carry on.
9. **Finish.** Say it is saved, that Madison will now load these notes in every chat in
   the workspace, and that to correct one later they just tell Madison in chat. If they
   started from Madison's onboarding page, tell them to go back to it in their browser:
   it moves on by itself. Then end with one line naming the next step, `create-problem`,
   to write up the first problem they want to solve: Claude Code `/madison:create-problem`;
   Cowork or Cursor `/create-problem`; Codex `$create-problem`; any other agent, name the
   skill in words.
