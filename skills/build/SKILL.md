---
name: build
description: Build a decided Madison Task or Bug - read the ticket, its comments and parents, set it In Progress, then implement it the way the team's own agent files say. Use when the user says to build, implement or execute a ticket whose open questions are answered.
---

This is Madison skills 0.3.6.

# Build a ticket

Turn a decided ticket into working code. How your team branches, reviews, commits and
raises changes is not this skill's to decide, and not Madison's: it comes from the team's
own agent files (CLAUDE.md, AGENTS.md or whatever your agent reads).

## Talk like this

- Short and plain. Lead with what changed.
- Check before you assert: every claim about the code names the file it came from.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## Steps

1. **Load the brief.**
   - `get_setup`: the team's answers, team rules and launch stage. Pre-live, there are no
     users to protect, so add no caution for them.
   - Pass `skillsVersion` to `get_setup`: the version in the "This is Madison skills" line
     near the top of this file (an unbuilt copy has none: pass nothing). If the answer's
     `skillsUpdate` is set, show it, then your agent's update route from `version-check.md`
     in full, word for word (every command, link and click, never a summary), then carry on.
   - The ticket, with `get_task` or `get_bug` and `includeSuperseded: true`. A Bug is its
     own implementation ticket: never cut a Task to do a Bug's work. A Task's response
     embeds its Initiative and Problems, and `resolvedDesignLink`.
   - `list_comments` on the ticket and on each parent it embeds. The ticket wins over a
     comment.
   - **Open questions still on the ticket:** say so and ask whether to answer them first
     (`start-ticket`) or build anyway. Never decide one silently.
   - **A design link** you can open: read it and build from what it shows. One you cannot
     open: stop and say so, naming the link. A missing link is fine.
2. **Set it In Progress** with the matching `update_*` tool, and its parents that are
   still in Backlog. Never move a Done or Closed parent: that would reopen it.
3. **Plan from the ticket.** The plan is `implementationSteps` when it has some, otherwise
   the approach plus the acceptance criteria. Active decisions are binding, and a
   superseded one is never brought back. If the code shows a decision cannot work, stop
   and say so rather than building around it.
4. **Build it the way the team works.** Every engineering choice (branching, code
   review, committing, raising the change for merge) comes from the team's own agent
   files, and nothing else. Madison's team rules cover ticket work, and hold too.
   - Do what the files say, and add no process they do not name. A team with no review
   gets none.
   - When they do not say something you need, ask the user, and suggest they add the
     answer to their agent files.
   - Follow the code's existing patterns, and run the project's own tests before handing
     off.
   - Write no `testingSteps`: the test plan is `verify`'s.
5. **Write back what you learned.** A choice made while building is a `decisions` entry
   ("We decided …", with `chosenAgainst`); something the code showed is a `findings` entry
   ("We found …"). Work that spans several tickets updates each one on its own.
6. **Hand off.** Say what was built and where it stands. End your last message with one
   line naming the next step: `verify`, in the agent the team's `agentRoles` gives it to
   (only that agent's command; the Claude Code and Cowork forms both only when
   `agentRoles` is blank), with the exact command for that agent: Claude Code
   `/madison:verify <ticket>`; Cowork or Cursor `/verify <ticket>`; Codex `$verify <ticket>`;
   any other agent, name the skill in words. No advice about which session to use.

Never mark the ticket Done here. That is `ship`'s call.
