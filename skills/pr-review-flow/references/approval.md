# Approval and hold rules

Use these rules to decide whether a PR is ready for approval. Submitting approval still needs the user's instruction; posting findings alone does not grant it.

## Approve

- Approve only the exact commit checked. Before submitting, fetch the current head and compare it with the reviewed commit. Tie the approval to the reviewed commit explicitly, and record the returned review link and commit. If the head changed, check the new changes first. Any later push needs a fresh look at its delta; repository settings may also drop the earlier approval.
- Leave no blocking point open. Remaining points must be optional suggestions or nits, and the approval must say they do not hold up merging.
- Judge behavior, not wording. An outdated PR description, document or code comment is at most a nit, never a reason to hold.
- Leave reasonable design choices to the author. If the author has explained a choice and it has no concrete blocking defect, trust it. Do not hold approval for the reviewer's preferred version.
- Weigh a proposed fix against the harm it could cause. A rare problem may remain when the fix would break more valuable behavior or cause greater harm. Explain the accepted trade-off; “rarely twice” can beat “rarely never.”
- For dependent PRs, check them together and, when approval is authorized and each is ready, approve both. State the merge order with PR links in each approval and in the tracker. A dependency alone is not a reason to hold approval; it sets the order for merging.

## Hold

Hold when a requested fix has not arrived, a question to the author has no answer, a decision only the user can make remains open, new commits have not been checked, or the PR has not been reviewed at all. Name what remains and who owns the next action.

Mark suggestions and nits as optional from the start. If a requested fix or question is settled by the author's explanation or an accepted trade-off, record that outcome so it does not remain a false blocker. Do not count a reasonable, explained design choice as an unanswered decision.

## Comments and follow-up

Each comment must stand alone. Name the issue in plain words and link to the code; never refer to private numbering such as “finding 4.” Follow the task review skill's rules for context, evidence and possible fixes.

Mark each point **must fix**, **suggestion**, or **nit (no action needed)**. Check existing comments, review threads and bot reviews before posting. Do not repeat a point already raised; add to its thread only when there is new evidence or a useful clarification.

After checking a fix, acknowledge it briefly, then list only what remains. Keep resolved points in the tracker without presenting them as open. An approval with only optional points left should say so.
