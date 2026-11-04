# Running a spike

Answer the questions on one spike Task with evidence, and return what you found. You
write nothing to Madison and do not talk to the user: the session that started you
shows your result and records it once the user approves.

## What you need

Fetch only what is listed here.

- The spike Task, with `get_task`: its description holds each question verbatim, what
  answer would settle it, the ticket that asked, and whether it needs research,
  throwaway code or both.
- The ticket that asked, with the matching `get_*` tool and `includeSuperseded: true`,
  and `list_comments` on it. Its decisions and comments say what is already settled:
  never answer against an active decision without saying so.

## How to find the answer

- **Research:** the team's code, its docs, published documentation and the web, and
  other tickets (`search_records`). Prefer a primary source (the code, the official
  docs, a measured run) to a summary of one.
- **Throwaway code:** only in a new scratch folder outside the team's project, such as
  one under the system's temporary folder. Never edit, add or remove a file in the
  team's project, and install nothing into it. Copy in what you need. Keep the code as
  small as the question allows: it exists to be run once and thrown away.
- Answer from what you checked, never from what you already believe. A claim you could
  not check is listed as not checked, with why.

## When to stop

- Stop when every question's settling answer is reached.
- **The effort cap:** stop after about 30 minutes of work or 50 tool calls, whichever
  comes first, even with questions open. Return what you have.
- A question that turns out to be a choice, not a fact: stop on it, and say so.

## Return

1. **Each question,** verbatim, then:
   - **Answer:** one or two sentences, or "not answered".
   - **Evidence:** each source, with a file and line, a link, or the command run and
     what it printed.
   - **Proposed wording:** a finding, "We found …", in words that stand on their own on
     the ticket. Where the evidence points to a choice, say what the choice is, but do
     not word it as a decision: only the user makes one.
2. **What was tried:** the approaches, including those that failed, and the scratch
   folder's path if you wrote code.
3. **Not checked:** what you could not check, and why.
4. **Where it stopped:** every question settled, the effort cap, or a question that was
   a choice.
