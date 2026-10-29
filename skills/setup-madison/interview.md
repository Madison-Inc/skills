# Interview

How to work a list of open questions with the user. The skill that sent you here supplies
the questions; this file says how to ask them.

## Before the first question

- Read the launch stage from `get_setup` if you have not already. Pre-live means nobody
  uses the product yet: never recommend caution, migrations, flags or warnings for users
  who do not exist.
- Say how many questions there are, then ask only the first.

## Each question

1. **Look it up first.** If the code, the ticket, its comments or a related ticket
   (`search_records`) can answer it, answer it yourself and say where the answer came
   from. Ask only what is genuinely the user's call.
2. **Ask in plain prose.** The options in plain words (A, B, C), then **a clear
   recommendation** and the one or two reasons that decide it. Rank by the best outcome,
   never by how much work it is. Never use a multiple-choice question dialog or form.
3. **One question per message.** If a question bundles several, say so and split it.
4. **Wait** for the answer before the next question.

## Recording an answer

- Write it to the ticket as soon as it is given, not in a batch at the end.
- A choice is a `decisions` entry: `statement` begins "We decided …", `chosenAgainst` is
  the option turned down (or why nothing was weighed). Then remove the answered open
  question with `removeOpenQuestions`.
- Something learned that new evidence could overturn is a `findings` entry ("We found …").
- Status, priority, links and how work is split into tickets are ticket admin: set the
  field, or post a comment. Never a decision.
- When the user's wording is not obvious, show the exact text before writing it.

## Stop

Stop when every question is answered or the user says so. Then check the ticket's title
and description still match what was decided; if not, propose new wording.
