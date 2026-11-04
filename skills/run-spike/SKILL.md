---
name: run-spike
description: Answer a Madison ticket's open questions that need evidence rather than a choice - research or throwaway code - as a Task labelled spike, then write each answer back onto the ticket as a finding or decision once the user approves. Use when start-ticket or create-initiative names it, or when the user asks to spike, prototype, research or find out something before deciding.
---

This is Madison skills 0.4.0.

# Run a spike

Some open questions are not the user's call: they need evidence first. A spike gets it,
records it on a Task labelled `spike`, and writes the answer back onto the ticket that
asked. The spike runs in a **fresh subagent** given only the spike Task's id, so this
session never holds the reading. If your agent has no subagents, run step 5 yourself,
from the spike Task's id alone.

## Talk like this

- Short and plain. Lead with the answer.
- Every claim names its evidence: a file, a page, a run of the throwaway code, a ticket.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## These rules hold every time

- **Evidence, not a choice.** "Can the search return 50,000 tickets in under a second?"
  needs evidence: spike it. "Should we build search or filters first?" is a choice: it
  goes back to `start-ticket`'s questions. A question with both parts: spike the fact,
  and the choice stays open.
- **Open Initiatives and Tasks only.** A spike Task links to an Initiative. A Problem
  waits for one, unless one is already linked to it. A Bug's cause is `explore`'s job.
- **The spike runs in a fresh subagent** given only the spike Task's id and step 5's
  instructions, or in this session from that id alone when there is no subagent.
- **Throwaway code goes only in a scratch folder outside the team's code.** Never edit,
  add or remove a file in the team's project, and install nothing into it.
- **Nothing is written to the ticket that asked until the user approves the exact
  wording** of each answer. The spike Task itself is saved straight away.
- **An answer is a finding ("We found …") unless the user makes a choice with it.** Only
  the user makes a decision ("We decided …", with what it was chosen against).

## Steps

1. **Load the brief.**
   - `get_setup`, passing `skillsVersion`: the version in the "This is Madison skills"
     line near the top of this file (an unbuilt copy has none: pass nothing). If the
     answer's `skillsUpdate` is set, show it, then your agent's update route from
     `version-check.md` in full, word for word (every command, link and click, never a
     summary), then carry on.
   - The ticket the user named, with its `get_*` tool. Given only words, use
     `search_records`, show the best match and confirm it. Done or Closed: say so, stop.
2. **Check the ticket and the questions.**
   - **The Initiative** is the ticket itself, a Task's Initiative (in `get_task`), or for
     a Problem one linked to it, which you offer. None, or declined: stop.
   - **A Bug:** write nothing, and stop.
   - Say which open questions you will spike and which are choices, by the first rule.
     The user can add or drop one. None needs evidence: say so, and stop.
3. **Look for a spike already open:** `list_tasks` with the Initiative's `initiativeId`
   and `label: "spike"`, then `search_records` with the questions' words. One asking the
   same thing: offer to resume it. Resumed, add any of this run's questions it lacks to
   its description and acceptance criteria (`update_task`), then go to step 5.
4. **Save the spike Task** with `create_task`, no draft: `title` "Spike: " and the
   question, shortened; `labels` `["spike"]`; `linkedInitiativeId` the Initiative;
   `areaId` the ticket's Area; `status` In Progress. A `userStory` naming who waits on
   the answer and what it lets them decide. A `description` with the ticket that asked,
   each question **verbatim** with what answer would settle it, and whether it needs
   research, throwaway code or both. `acceptanceCriteria`: one per question, "<the
   question> has been answered". Set the Initiative, and its Problems, In Progress if
   they are in Backlog. Reply with the link (`create-ticket.md` gives the format).
5. **Run the spike in a subagent.** Give a fresh subagent the spike Task's id and these
   instructions. It returns text, does not talk to the user and does not write to Madison.
   - Run the spike in `spike.md` in this skill's folder, and return what it says to
     return.
6. **Show the result** as it came back: answers, evidence, what was tried, what was not
   checked, and where it stopped. Then take each answer in its own message, with the
   exact wording you propose for the ticket: approve, change or drop. Wait for each.
7. **Once approved,** in this session:
   - Each approved answer onto the ticket that asked, as `findings` or `decisions`, with
     its `update_*` tool. Remove the question it answers with `removeOpenQuestions`, its
     text exactly as on the ticket; a question that was half choice gets the choice back
     as a new open question. A dropped answer leaves its question open.
   - One comment on the spike Task (`create_comment`): every answer, the evidence, what
     was tried, what was not checked, and what was written to the ticket.
   - Close the spike Task as the team's `doneWhen` and team rules say, **only once it has
     no open questions**. `doneWhen` blank: ask it first, recommend an answer and save it
     only once the user accepts, as `setup-question.md` in this skill's folder says. Clear
     any it carries first, one at a time: answered (a
     `decisions` or `findings` entry, then `removeOpenQuestions`), moot (removed, saying
     why), or moved to the open ticket (not Done or Closed) that will answer it
     (`openQuestions` there, then `removeOpenQuestions` here, its text exactly as on this
     ticket). Any still open: leave it In Progress and name each.
     When `doneWhen` and the team rules do not cover spikes, ask once whether a spike
     with approved answers is Done or Closed, set it, and offer to save the answer as a
     team rule (`update_setup` with `addRules`). Refused for lack of role: say an owner or
     admin can add it.
8. **Hand off.** End your last message with one line naming the next step, in the agent
   the team's `agentRoles` gives it to (only that agent's command; `agentRoles` blank: ask
   it first, recommend an answer and save it only once the user accepts, as
   `setup-question.md` in this skill's folder says; declined, give both the Claude Code
   and Cowork forms): `start-ticket` on the spike Task
   when open questions kept it open; `start-ticket` on the ticket that asked while it has
   open questions; otherwise `create-task` for an Initiative (or `create-design` first,
   when `designTool` is set and not "none yet", the Initiative has no `designLink` and
   its `inScope` changes anything users see; blank `designTool` counts as none),
   `explore` or `build` for a Task (`build` once explored), and `start-ticket` on the
   Initiative for a Problem. Stopped at step 2: `create-initiative` for a Problem with no
   Initiative, `explore` for a Bug. Claude Code `/madison:<skill> <ticket>`; Cowork or
   Cursor `/<skill> <ticket>`; Codex `$<skill> <ticket>`; any other agent, name the skill
   in words. No advice about which session to use.
