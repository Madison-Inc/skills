# The setup questions

Read this during step 3 of setup-madison. Each field is free text: save the team's own
words, short. The examples are examples, not defaults; never save one the user did not
choose.

| Field | Ask | Example answers |
| --- | --- | --- |
| `doneWhen` | When is a ticket Done? | "merged to main"; "live in production"; "deployed to staging" |
| `agentRoles` | Which agent does what? | "Cowork shapes tickets and reviews, Claude Code explores and builds"; "one agent does everything" |
| `designTool` | Where do designs live? | "Figma"; "Claude Design"; "none yet" |
| `retroWhen` | When should a retro run? | "after each Initiative is Done"; "only when asked" |

## Team rules

After the fields, ask: "Is there anything your team always or never does with tickets
that an agent should know?" Save each one as its own rule with `addRules`, in the team's
words, e.g. "Never reopen a ticket; a defect that comes back gets a new one" or "Ask
questions as a comment on the ticket".

Rules are for ticket work only. How the team branches, reviews, merges or releases, and
facts about the code, belong in the repository's own agent files and docs, not here.
