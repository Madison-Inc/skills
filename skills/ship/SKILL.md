---
name: ship
description: Close out a verified Madison Task or Bug - check whether the team's definition of Done is met, mark the ticket Done only then, and close the chain (parent statuses, leftover open questions, whether the Problem is solved). Safe to run more than once. Use when the user says to ship, close or finish a ticket.
---

This is Madison skills 0.1.7.

# Ship a ticket

End a ticket properly. How your team merges and releases is not this skill's to decide,
and not Madison's: it does only the release steps the team's own agent files (CLAUDE.md,
AGENTS.md or whatever your agent reads) give the agent, and checks Done against the team's
`doneWhen` from the Madison setup. Run it as often as you like; each run picks up where things stand.

## Talk like this

- Short and plain. Lead with whether the ticket is Done.
- Check before you assert: say how you know each part of `doneWhen` is met.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## Steps

1. **Load the brief.**
   - `get_setup`: the team's answers, team rules and launch stage.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it in one line with your agent's update route from
     `version-check.md`, then carry on.
   - The ticket, with `get_task` or `get_bug` and `includeSuperseded: true`, and
     `list_comments` on it. The test results are in its comments.
2. **Do only what the team's agent files give the agent.** If they assign a release step
   to the agent at this point, do it. Never merge unless they say the agent may.
3. **Check `doneWhen`.** Check each part you can reach yourself, and ask the user about the
   rest. If it is blank, ask the user when a ticket is Done, and suggest `setup-madison` to
   save it.
   - **Not met:** say what is outstanding and who acts next, and stop. The ticket stays
     In Progress.
4. **Re-read every acceptance criterion** against what shipped: the code, and the test
   results on the ticket. Say met or not met for each, with how you know.
   - **All met:** mark the ticket Done with the matching `update_*` tool.
   - **Any not met:** say which, and leave the ticket as it is. Ask whether to fix it under
     this ticket or capture it as a new ticket.
5. **Close the chain**, only when every acceptance criterion is met. Set these yourself,
   without asking, and say what you changed. A Bug has no parents: go straight to its
   leftover open questions.
   - **The Initiative:** Done once every Task linked to it is Done or Closed **and** no
     work in its scope is still to be written as a Task (an Initiative building an MVP
     first writes its other Tasks only after the MVP passes). Otherwise leave it, and say
     what is left.
   - **Each Problem it solves:** Done once every Initiative linked to it is Done or Closed
     and nothing on the Problem or its thread says the pain remains. Otherwise leave it,
     and say why.
   - **Leftover open questions** on the ticket and on anything you marked Done: work them
     one at a time, as `interview.md` in this skill's folder says. Each is answered (a
     `decisions` entry), moot (removed), or still open (moved to the ticket that will
     answer it).
6. **Hand off.** End your last message with one line naming the next step, in the agent
   the team's `agentRoles` gives it to (only that agent's command; the Claude Code and
   Cowork forms both only when `agentRoles` is blank):
   - **The Initiative is now Done:** a retro on it, when the team's `retroWhen` says one
     is due. Name it in words: there is no retro skill yet.
   - **Otherwise:** the next ticket under the Initiative, picked up with `start-ticket`, or
     `ship` again once whatever was outstanding is done.

   Give the exact command where there is a Madison skill for it: Claude Code
   `/madison:<skill> <ticket>`; Cowork or Cursor `/<skill> <ticket>`; Codex
   `$<skill> <ticket>`; any other agent, name the skill in words. No advice about which
   session to use.
