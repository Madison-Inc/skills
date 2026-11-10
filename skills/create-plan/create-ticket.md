# Creating a ticket

Read this during a create skill. The skill's own SKILL.md holds the rules that must hold
every time; this file holds the detail behind them.

## Search first

Call `search_records` with the words you are about to write, before any `create_*` tool.
Read the score, not the position:

- **0.8 or more:** near-verbatim. It is almost certainly the same ticket: show it and
  offer to update or comment on it instead.
- **0.5 to 0.8:** worth opening. Fetch it and compare before deciding.
- **Below 0.3:** usually a shared common word. Ignore it.

A no-match on one phrasing is weak evidence. If it matters, search once more with other
words. If you are unsure whether a match is the same ticket, say so and ask; never create
quietly past one.

## Which Area

Create the ticket in the Area it belongs to:

- **It has a parent** (a Task's Initiative, an Initiative's Problem): use the parent's
  Area, from the parent's `area` in its `get_*` response. Pass it as `areaId`.
- **No parent:** pass `cwd` (your working directory) and Madison works the Area out from
  the folder.
- If Madison answers `AREA_REQUIRED`, show the Areas it lists, ask which one, and retry
  with `areaId`.

## Fields

- Always send `description`: the prose behind the one-line title or statement.
- Fill a field only with what the conversation, the code or a parent ticket gives you.
  An empty field is better than an invented one; an unknown is an open question.
- Open questions belong at their level: discovery (who, how bad, why it matters) on a
  Problem, approach (which way to solve it) on an Initiative, implementation (how to
  build it) on a Task or Bug.
- Decisions already made go in `decisions` (`statement` starting "We decided …",
  `chosenAgainst` naming what it beat). Something learned goes in `findings` ("We
  found …").
- Severity and priority: the bands are on those fields in the tool. Judge severity per
  occurrence; frequency and urgency are priority.

## The link

The create response holds the ticket's `ticketId` and its `area`. The ticket's page is:

- with an Area: `https://app.madison.works/dashboard/a/<area.key>/<type>/<ticketId>`
- without one: `https://app.madison.works/dashboard/<type>/<ticketId>`

where `<type>` is `problems`, `initiatives`, `tasks` or `bugs`. Reply with it as a
Markdown link, the ticket id as its text.
