---
name: verify
description: Check a built Madison Task or Bug - write a tagged test plan onto the ticket, say where the change can be seen, hand the tests to whoever the team's agent files name, and record the results. Use when the user says to verify, test or check a ticket after it is built.
---

This is Madison skills 0.3.6.

# Verify a ticket

Prove the change does what the ticket says, and leave the proof on the ticket. Who runs
the tests and where the change can be seen come from the team's own agent files (CLAUDE.md,
AGENTS.md or whatever your agent reads), not from Madison.

## Talk like this

- Short and plain. Lead with what passed and what did not.
- Check before you assert: a result names the step it came from.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## Steps

1. **Load the brief.**
   - `get_setup`: the team's answers, team rules and launch stage.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - The ticket, with `get_task` or `get_bug`, and `list_comments` on it. A Task's
     response embeds its Initiative.
2. **Say where the change can be seen**, from the team's agent files, as a comment on the
   ticket (`create_comment`), in plain words with the link or place. If they do not say,
   ask the user, and suggest they add it to their agent files. Do this first: the test
   plan points at this comment.
3. **Write the test plan.** Read `test-plan.md` in this skill's folder for the detail.
   These rules hold every time:
   - The first entry is one `[SETUP]` block: target, auth, preconditions, test data.
   - Every later entry is **one** step starting with **exactly one** of `[UI]`, `[API]`,
     `[DB]` or `[MANUAL]`. A `[UI]` step reads `Action: … Expect: …` and checks only what
     the user can see.
   - Every acceptance criterion gets at least one step.
   - Save the whole plan with `replaceTestingSteps`: the plain `testingSteps` field
     appends and skips duplicates, so an old plan would linger.
4. **Hand the tests to whoever the team's agent files say checks a change.**
   - **They name one person, agent or tool:** hand it to them.
   - **It depends on the ticket** (the files say it is decided per ticket, or name a rule such
     as "the product lead tests big UI changes"): say which you think applies to this
     ticket and why, then ask. Wait for the answer.
   - **You run them:** run every step you can reach and report each one. A step you cannot
     reach goes to the user, named.
   - **Someone else runs them:** show the full numbered plan in chat, say where to run it,
     and wait for their results.
   - **They do not say:** ask the user who tests this ticket.
5. **Record the results** as one comment on the ticket: each step, pass or fail, and what
   was seen for a failure. A failure is not this skill's to fix: it goes back to `build`.
   If the ticket is the MVP an Initiative is waiting on (the Initiative decided to build an
   MVP first), also record pass or stop as a `findings` entry on the Initiative ("We found
   …"), against the bar the ticket's success metric sets.
6. **Hand off, only once the results are recorded.** While tests are still to run, say
   what you are waiting for and name no next step. Then end your last message with one
   line naming the next step, in the agent the team's `agentRoles` gives it to (only that
   agent's command; the Claude Code and Cowork forms both only when `agentRoles` is
   blank): `ship` once every step has passed, or `build` to fix a failure. Give the exact
   command: Claude Code `/madison:<skill> <ticket>`; Cowork or Cursor `/<skill> <ticket>`;
   Codex `$<skill> <ticket>`; any other agent, name the skill in words. No advice about
   which session to use.

Never mark the ticket Done here. That is `ship`'s call.
