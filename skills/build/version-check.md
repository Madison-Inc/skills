# Updating the Madison skills

When `get_setup` returns a `skillsUpdate` line, show it once, in one message, followed by
the update route for the agent you are running in. Then carry on with the skill: an
out-of-date skill still runs. If you cannot tell which agent you are, give all three.

- **Claude Code:** run `claude plugin marketplace update madison`, then
  `claude plugin update madison@madison`, then `/reload-plugins` (or start a new session).
  To stop seeing this, turn on auto-update: `/plugin` → Marketplaces → madison → Enable
  auto-update.
- **Cowork:** download `madison-<version>.plugin` from
  https://github.com/Madison-Inc/skills/releases/latest and upload it in Cowork's plugin
  settings.
- **Other agents** (installed with `npx skills`): run `npx skills update`.
