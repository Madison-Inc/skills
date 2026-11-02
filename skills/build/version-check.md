# Updating the Madison skills

When `get_setup` returns a `skillsUpdate` line, show it once, in one message, followed by
the update route for the agent you are running in. Give that route in full, word for word
as written below: every command, link and click. Never summarise it ("reinstall the
plugin" is not a route). Then carry on with the skill: an out-of-date skill still runs. If
you cannot tell which agent you are, give all three.

- **Claude Code:** run `claude plugin marketplace update madison`, then
  `claude plugin update madison@madison`, then `/reload-plugins` (or start a new session).
  To stop seeing this, turn on auto-update: `/plugin` → Marketplaces → madison → Enable
  auto-update.
- **Cowork:** on the Plugins page, click **+ Add** → **Manage marketplaces**, then **⋯**
  next to **skills** → **Check for updates**. Then start a new task.
- **Other agents** (installed with `npx skills`): run `npx skills update`.
