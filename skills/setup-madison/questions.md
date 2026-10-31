# The setup questions

Read this during step 3 of setup-madison. Each field is free text: save the team's own
words, short. The examples are examples, not defaults; never save one the user did not
choose.

| Field | Ask | Example answers |
| --- | --- | --- |
| `branchNaming` | How do you name branches? | `feature/<ticket>-<slug>`; `<your-name>/<ticket>-<slug>` |
| `releaseFlow` | How does a change get to production? | "PR to main, deploys on merge"; "PR to staging, then staging is promoted to main" |
| `whoMerges` | Who merges a pull request? | "the author once checks pass"; "the product lead"; "a reviewer" |
| `doneWhen` | When is a ticket Done? | "merged to main"; "live in production"; "deployed to staging" |
| `whereToSeeChange` | Where can a change be seen before it ships? | "a Vercel preview per PR"; "staging"; "locally only" |
| `whoChecks` | Who reviews the code, and who tests a change before it ships? Is that the same every time, or decided per ticket? | "no review; the author tests"; "a review agent checks the diff, QA runs the test plan"; "a teammate reviews the PR, the product lead tests the preview"; "decided per ticket" |
| `agentRoles` | Which agent does what? | "Cowork shapes tickets and reviews, Claude Code explores and builds"; "one agent does everything" |
| `designTool` | Where do designs live? | "Figma"; "Claude Design"; "none yet" |
| `retroWhen` | When should a retro run? | "after each Initiative ships"; "end of each sprint"; "only when asked" |

## Team rules

After the fields, ask: "Is there anything your team always or never does that an agent
should know?" Save each one as its own rule with `addRules`, in the team's words, e.g.
"Every PR links its ticket" or "Never push to main directly".

Rules are for how the team works. Facts about the code belong in the repository's own
docs, not here.
