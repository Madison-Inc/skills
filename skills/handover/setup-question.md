# Asking a setup question

The team setup holds four answers (below). A new team starts with all four blank, and no
skill fills one in for them. When a step needs an answer that `get_setup` returns as null,
ask for it then, recommend one, and save it only once the user accepts it.

## These rules hold every time

- **Ask when a step needs the answer, never before.** Ask only that one question, in its
  own message, in plain prose (never a multiple-choice dialog or form), and wait for the
  reply. Ask each question at most once per run.
- **Recommend, never assume.** Give one recommended answer and the one reason for it.
  Base it on what you can see: the team's agent files (CLAUDE.md, AGENTS.md…), which agent
  you are and which others the user has mentioned, the launch stage, and the other
  answers. With nothing to go on, recommend the fallback in the table.
- **Save only what the user accepts.** They accept: save your recommendation. They change
  it: save their words, short. Use `update_setup` with just that field, straight away,
  then carry on with the step that needed it.
- **They decline or skip it:** save nothing. Say in one line what you cannot check or
  decide without it, then carry on as the skill says for a blank answer.
- **`update_setup` refuses** (only an owner or admin can save the setup): use the answer
  for this run only, and say an owner or admin can save it.
- Never suggest running `setup-madison` just because an answer is blank. It stays for
  changing answers later, or filling several at once.

## The four questions

Each answer is free text in the team's own words. The examples are examples, not
defaults.

| Field | Ask | Example answers | Fallback recommendation |
| --- | --- | --- | --- |
| `doneWhen` | When is a ticket Done? | "merged to main"; "live in production"; "deployed to staging" | "merged to the main branch" |
| `agentRoles` | Which agent does what? | "Cowork shapes tickets and reviews, Claude Code explores and builds"; "one agent does everything" | "this agent does everything", naming the agent you are |
| `designTool` | Where do designs live? | "Figma"; "Claude Design"; "none yet" | "none yet" |
| `retroWhen` | When should a retro run? | "after each Initiative is Done"; "only when asked" | "after each Initiative is Done" |

## Using `agentRoles` in a hand-off

Name the next step's command only for the agent `agentRoles` gives it to. Still blank
after asking (declined): give both the Claude Code and the Cowork forms.
