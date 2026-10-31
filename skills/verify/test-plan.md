# The test plan

How to write a ticket's `testingSteps`. It is an array of plain-language strings, read by
people and by any tool that runs tests for your team. Follow the tags and shapes exactly:
a tool may rely on them.

## 1. The first entry is a `[SETUP]` block

One string, starting `[SETUP]`, holding everything a run needs before step 1:

- **Target:** where the change can be seen, from the team's `whereToSeeChange`. If the
  link is only known once it exists (a preview deploy, say), point to the comment on this
  ticket that gives it, rather than writing a link down that may change.
- **Auth:** reuse the tester's own signed-in session. Never automate a sign-in or write a
  credential into the plan.
- **Preconditions:** the state the plan assumes: which account or workspace, what data
  must exist, what must be empty.
- **Test data:** any input a step types word for word, given exactly.

```
[SETUP] Target: the preview link posted as a comment on this ticket. Auth: reuse the tester's signed-in session. Preconditions: a workspace with at least one open ticket. Test data: search text A = "billing export".
```

## 2. Every later entry is one tagged step

Each string is **one** step, starting with **exactly one** tag. One action and one check
per step; never bundle.

| Tag | Meaning |
| --- | --- |
| `[UI]` | An action in the product's interface, and what the user then sees |
| `[API]` | A request, and the response it should get |
| `[DB]` | A check of stored data |
| `[MANUAL]` | Anything that needs a person's judgement |

A team with an automated runner usually runs `[UI]` steps and leaves the rest to a person
or other tooling.

## 3. `[UI]` steps check what the user sees

```
[UI] Action: <do exactly this>. Expect: <one result the user can see>.
```

Check visible labels, on-screen text and roles. Never check CSS classes, colour values or
page structure: nobody using the product sees them, and they break on a restyle.

- Good: `Expect: a "Saved" message appears and the row shows "Quarterly review".`
- Bad: `Expect: the panel has the class .surface-page.`

## 4. AI output: split what is fixed from what is judged

When a step involves AI-generated output, split it in two:

- **What is fixed** (a response came back, a tool was called, a record was created with
  the right type and parent) is a `[UI]`, `[API]` or `[DB]` step. Check a created record
  in the data, not by reading the AI's text.
- **What is judged** (the wording, the quality of an answer) is `[MANUAL]`. Never assert
  on what the AI says: it changes from run to run.

A step that sends input to the AI gives the **exact** text to send, so every run is the
same.

## 5. Order: one session, top to bottom

The plan runs in one session, in order. Any state a later step needs (an open page, a
record just made) must still hold when it runs. Keep dependent steps together, and never
put a step that navigates away between them.

## Example

```
[SETUP] Target: the preview link posted as a comment on this ticket. Auth: reuse the tester's signed-in session. Preconditions: a workspace with two tickets titled "Alpha" and "Beta". Test data: none.
[UI] Action: open the ticket list and type "Alp" in the search box. Expect: only "Alpha" is listed.
[UI] Action: clear the search box. Expect: both "Alpha" and "Beta" are listed.
[API] Action: request the ticket list with search "Alp". Expect: one result, titled "Alpha".
[MANUAL] Action: look at the empty-search state. Expect: it reads clearly (judgement).
```
