---
name: ship
description: Close out a verified Madison Task or Bug - check whether the team's definition of Done is met, work every open question left on the ticket, mark it Done only once none is left, and close the chain (parent statuses, each parent's own open questions, whether the Problem is solved). Safe to run more than once. Use when the user says to ship, close or finish a ticket.
---

This is Madison skills 0.6.0.

# Ship a ticket

End a ticket properly. How your team merges and releases is not this skill's to decide,
and not Madison's: it does only the release steps the team's own agent files (CLAUDE.md,
AGENTS.md or whatever your agent reads) give the agent, and checks Done against the team's
`doneWhen` from the Madison setup. Run it as often as you like; each run picks up where things stand.

## Talk like this

- Short and plain. Lead with whether the ticket is Done.
- Check before you assert: say how you know each part of `doneWhen` is met.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **An open question blocks Done**, on the ticket, its Initiative and each Problem. Before
  any of them is marked Done, each open question on it is cleared one of three ways:
  - **Answered:** a `decisions` or `findings` entry, then `removeOpenQuestions`.
  - **Moot:** removed with `removeOpenQuestions`, saying why.
  - **Moved** to the open ticket (not Done or Closed) that will answer it: `openQuestions`
    on that ticket, then `removeOpenQuestions` here, its text exactly as on this ticket.

  Any question still open: leave that ticket as it is, name each question left, and say
  who acts next.
- **Leave a parent that is already Done or Closed alone:** no status change, and no work
  on its questions.
- **ship only ever sets Done.** A ticket that is not Done stays as it is. A Plan is marked
  Done only on the user's yes to its question.

## Steps

1. **Load the brief.**
   - `get_setup`: the team's answers, team rules and launch stage.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - The ticket, with `get_task` or `get_bug` and `includeSuperseded: true`, and
     `list_comments` on it. The test results are in its comments.
2. **Do only what the team's agent files give the agent.** If they assign a release step
   to the agent at this point, do it. Never merge unless they say the agent may.
3. **Check `doneWhen`.** Check each part you can reach yourself, and ask the user about the
   rest. **If it is blank,** ask when a ticket is Done before checking anything, with a
   recommended answer, and save it only once the user accepts or changes it, as
   `setup-question.md` in this skill's folder says. They decline: save nothing, say Done
   cannot be checked without it, and stop with the ticket as it is.
   - **Not met:** say what is outstanding and who acts next, and stop. The ticket stays
     In Progress.
4. **Re-read every acceptance criterion** against what shipped: the code, and the test
   results on the ticket. Say met or not met for each, with how you know.
   - **All met:** go on to step 5.
   - **Any not met:** say which, and leave the ticket as it is. Ask whether to fix it under
     this ticket or capture it as a new ticket.
5. **Work the ticket's open questions**, one at a time, as `interview.md` in this skill's
   folder says, clearing each by the first rule. A question it says to leave open for
   `run-spike` stays open, so it blocks Done too.
   - **None left:** mark the ticket Done with the matching `update_*` tool.
   - **Any left:** name each, leave the ticket as it is, and go to step 7.
6. **Close the chain**, only once the ticket is Done. Set statuses yourself, without
   asking, and say what you changed. A Bug has no parents: go to step 7. Work the
   Initiative first, then each Problem.
   - **The Initiative:** once every Task linked to it is Done or Closed **and** no work in
     its scope is still to be written as a Task (an Initiative building an MVP first writes
     its other Tasks only after the MVP passes), work its open questions as in step 5, then
     mark it Done once none is left. Otherwise leave it, and say what is left.
   - **Each Problem it solves:** once every Initiative linked to it is Done or Closed and
     nothing on the Problem or its thread says the pain remains, work its open questions
     as in step 5, then mark it Done once none is left. Otherwise leave it, and say why.
7. **Ask about each Plan that is ready.** When an `update_problem` or `update_bug` call in
   this run says a Plan the ticket is in is now ready (every ticket in it Done or Closed),
   ask about that Plan once, however many tickets named it, one Plan per message: say in
   one line that every ticket in it is Done or Closed, and ask whether its outcome was
   reached.
   - **Yes:** `update_plan` with status Done.
   - **No:** ask what is still missing, and add it to the Plan's `unknowns` with
     `update_plan`.
   - **No update named a Plan:** go on.
8. **Hand off.** End your last message with one line naming the next step, in the agent
   the team's `agentRoles` gives it to (only that agent's command; `agentRoles` blank: ask
   it first, recommend an answer and save it only once the user accepts, as
   `setup-question.md` in this skill's folder says; declined, give both the Claude Code
   and Cowork forms). The line names the step that is actually next:
   - **A change is still waiting on a merge or release step** the team's agent files give
     to someone else (this ticket's, or one the agent raised in this session that is still
     unmerged): that step comes first, and who does it (e.g. "merge the PR"), never `ship`
     or `start-ticket`. Name nothing after it. The cases below apply only once nothing is
     waiting.
   - **This run marked the Initiative Done:** `retro` on the Initiative when the team's
     `retroWhen` says one is due; otherwise, say nothing is left on it. `retroWhen` blank:
     ask it, recommend an answer and save it only once the user accepts, as
     `setup-question.md` says; declined, offer `retro` without saying one is due. When open
     questions also kept a Problem open, the same line also names that Problem's next
     step, as below for a parent kept open.
   - **Open questions kept the ticket open:** `run-spike` on the ticket when one was left
     for a spike; otherwise `start-ticket` on the ticket. Then `ship` on it again.
   - **Open questions kept the Initiative or a Problem open:** `run-spike` on the
     Initiative when one was left for a spike; otherwise `start-ticket` on that ticket.
     Then `ship` on this ticket again, which picks the chain up where it stands.
   - **Otherwise:** the next ticket under the Initiative, picked up with `start-ticket`, or
     `ship` again once whatever was outstanding is done.

   Give the exact command where there is a Madison skill for it: Claude Code
   `/madison:<skill> <ticket>`; Cowork or Cursor `/<skill> <ticket>`; Codex
   `$<skill> <ticket>`; any other agent, name the skill in words. No advice about which
   session to use.
