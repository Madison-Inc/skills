---
name: review-comment
description: Explain another agent's comment on a Madison ticket (an exploration, a review, a proposal) in plain words with a recommendation for each open decision, then post the reply and record the decisions once the user approves. Use when the user asks what a comment means, or to review or answer one.
---

# Review a comment

A second opinion on work another session did. The review itself runs in a **fresh
subagent**, never in the session that wrote the comment, so it is not marking its own
homework. If your agent has no subagents, ask the user to run this skill in a new session.

## Talk like this

- Short and plain. Lead with the answer. No jargon without a plain gloss.
- Ask in plain prose, never with a multiple-choice question dialog or form.

## Steps

1. **Find the comment.** `list_comments` on the ticket the user names. The target is the
   latest agent comment unless the user names another. Note its id.
2. **Review it in a subagent.** Give a fresh subagent the ticket id, the comment id and
   these instructions, and have it return the review as text. It does not talk to the user
   and does not write to Madison.
   - Fetch the ticket with `includeSuperseded: true` (so it never recommends something
     already decided against), `get_setup`, and the thread for context.
   - Check each claim the recommendations depend on, in the code or in Madison, and mark
     any it could not check as the comment's claim, unverified.
   - Return three parts: **The problem** (two or three plain sentences); **The findings**
     (short bullets, each saying what it means for the user); **Decisions** (numbered:
     the question in one line, **Recommend: X**, one line of why, and where it disagrees
     with the comment's own recommendation). Rank by best outcome, never by effort.
     Pre-live, never recommend caution for users who do not exist.
3. **Show the review** to the user as it came back. Then wait. Do not write anything to
   the ticket until the user has answered.
4. **Once approved,** in this session:
   - Post one reply under the reviewed comment (`create_comment` with `parentId`) giving
     each answer, @mentioning the comment's author by the handle `list_comments` returned.
   - Record each answer as a `decisions` entry on the ticket ("We decided …", with
     `chosenAgainst`), and remove the open questions it answers. Ticket admin (status,
     priority, splitting work) goes in the reply, not in decisions.
5. **Hand off.** Say which agent picks it up next, from the team's `agentRoles` setup if
   answered, and that it belongs in a new session.
