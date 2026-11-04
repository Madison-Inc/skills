# Is the ticket still relevant?

Check one open ticket against three sources and return a verdict: **Done**, **Closed** or
**keep**. Every piece of evidence names its source: a commit, a file, a ticket id, or a
decision quoted as it reads. Anything you could not check is listed as unverified, never
guessed.

## What you need

Fetch only what you do not already hold.

- `doneWhen`: as the caller gives it, otherwise from `get_setup`. Given as "none", or
  still blank, it is blank: go by the agent files alone, and never ask for it here (that
  is the caller's job).
- The ticket, with the matching `get_problem`, `get_initiative`, `get_task` or `get_bug`
  and `includeSuperseded: true`, and `list_comments` on it and on each parent it embeds.
- From the team's agent files (CLAUDE.md, AGENTS.md…) and `doneWhen`:
  - **Where work counts as shipped**, usually a branch. If neither says, the code check
    is unverified.
  - **How the team links work to a ticket:** a commit prefix, a branch pattern, a PR
    title. Teams differ, so assume no format of your own.

## The three checks

1. **The code** (a Task or Bug; a Problem or Initiative is judged by its children
   instead). Bring the history up to date first. Then find the work linked to this
   ticket, by the team's convention, or, when none is stated, by the ticket id (in any
   case, never as part of a longer id: ABC-12 must not match ABC-120) in:
   - commit messages, merge commits included;
   - branch names;
   - a PR or commit linked in the ticket's comments. It counts only once you have
     confirmed it is merged where work counts as shipped. Many teams leave no such
     link, so never rely on one.

   With git, for example: `git fetch --all --quiet`;
   `git log --all --oneline -i -E --grep='<ticket id>([^0-9]|$)'`;
   `git branch -a --list` filtered for the id; then `git branch -a --contains <commit>`
   for which branches hold each commit.

   Then look for the work itself: the files, routes or behaviour the ticket asks for.
   Sort what you find:
   - work linked to this ticket, shipped;
   - work linked to this ticket, not shipped (name the branch, commit or PR);
   - the work shipped, linked to another ticket (name that ticket and commit);
   - the work shipped, linked to no ticket (name the commit or file);
   - nothing.

   No access to the code: say so, and mark the code check unverified.
2. **Other tickets.** `search_records` with the ticket's own words (its title or
   statement). On no strong match, try once more with different wording. Open any match
   scoring 0.5 or more. Find related tickets this way only, never by paging the `list_*`
   tools; the one list call is an Initiative's own Tasks, below. Look for a duplicate, a
   ticket that already did this work, or one that superseded it.
3. **The parents.** Read the active decisions on each parent the ticket embeds. A
   decision that rules out this ticket's work makes it moot. A superseded decision does not
   count.

## The verdict

**A Task or Bug:**

- **Done:** work linked to this ticket is shipped, and each acceptance criterion is met
  in the code. Say whether its comments hold passing test results.
- **Closed:** nothing was committed against it, and it duplicates another ticket, a
  parent's active decision makes it moot, another ticket superseded it, or its work
  shipped linked to another ticket. Name that ticket, decision or commit. A ticket with
  any work linked to it is never Closed.
- **Shipped but unlinked:** the work looks shipped, but nothing links it to any ticket.
  The verdict is **keep**, marked *shipped, unlinked*, naming what you found. It is never
  Closed. Check each acceptance criterion as for Done and say which are met: if the user
  confirms the work was this ticket's and all are met, it becomes Done.

**An Initiative or Problem**, judged by its children with `ship`'s rules:

- **Done:** an Initiative's Tasks (`list_tasks` with its `initiativeId`) are all Done or
  Closed, at least one is Done, **and** no work in its scope is still to be written as a
  Task. A Problem's Initiatives are all Done or Closed, at least one is Done, and nothing
  on the Problem or its thread says the pain is still there.
- **Closed:** no child is Done, and it duplicates another ticket, a parent's active
  decision makes it moot, or another ticket superseded it. Name that ticket or decision.

**Any type:**

- **keep:** anything else. The work is not shipped yet (name the branch, commit or PR),
  is only partly done, or is still needed. Give one line of why.
- **A check you could not make:** Done never rests on it; give keep. Closed may, when the
  duplicate, decision or superseding ticket is certain: list the unchecked code as
  unverified so the user can check it before approving.

## Return

The ticket id and type, its status, the verdict (and *shipped, unlinked* when it applies),
one line of why, the evidence (one item per line, each naming its source), what is
unverified, and, for a Task or Bug whose work is shipped, whether passing test results
are on the ticket. With a Done verdict, also the ticket's open questions, each word for
word as on the ticket.
