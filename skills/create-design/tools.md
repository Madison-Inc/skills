# Making a design in Claude Design

Read this during `create-design` when the team's `designTool` is Claude Design. Only
Claude Design is supported for now. Any other tool gets the hand-made-link path in the
skill's step 2.

## Does this agent have it?

A Claude Design is an artifact made from the **Design** artifact type: a canvas of live
artboards. Your agent has it when its artifact tool lists that type. In Claude, ask the
Artifact tool for a quickstart with the intent "design" (or list the artifact types), and
look for the core "Design" type.

- **It is listed:** carry on.
- **No artifact tool, or no Design type:** this agent can't make a Claude Design. Stop and
  hand off as the skill says. A plain artifact page is not a Claude Design: never make one
  in its place.

## Make it

1. Start a new artifact from the Design type. Title it with the ticket's short name, never
   the full Initiative statement.
2. Follow the type's own instructions for filling it. Give it the brief from the skill's
   step 4. Let the type pick the design system: pass none.
3. Keep the brief to the fields the design needs. Never paste the whole ticket, its
   comments or its decision history into the design.

## Revise one in place

Open the existing design by its link with the artifact tool's read action, then change it
by that same link the way the Design type's own instructions say (the type's page and
files stay fixed; only the design's own content changes). The link stays the same, so
the ticket needs no update.

## The link

The design's link is the artifact's https link, as the artifact tool returns it:
`https://claude.ai/artifact/…`, or `https://claude.ai/code/artifact/…` when it was made
from Claude Code. Both are valid design links. Open it with the artifact tool's read
action before you set it. If it does not open, do not set it.
