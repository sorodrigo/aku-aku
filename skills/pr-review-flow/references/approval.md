# Approval and hold rules

Use these rules to decide whether a PR is ready for approval. A task review or full review run authorizes the lead agent to approve ready PRs on the user's behalf without asking for confirmation, unless the user requests read-only work, drafts or no approvals. Approve only when the evidence gives confidence and no blocking point or required decision remains open. Subagent verdicts alone are not enough.

## Approve

- Approve only the exact commit checked. Before submitting, fetch the current head and compare it with the reviewed commit. Tie the approval to the reviewed commit explicitly, and record the returned review link and commit. If the head changed, check the new changes first. Any later push needs a fresh look at its delta; repository settings may also drop the earlier approval.
- Re-approve quickly after a small push. If the user approved an earlier commit and the new delta is only a merge or rebase from the base branch, or small changes that address review points, check just that delta at light depth: read the PR's own diff since the approved commit, confirm CI passes, and approve the new head with an empty body. For a merge, check only the conflict resolutions. Do a full review only when the delta adds new behavior, touches files the PR did not change before, or hits a deep-depth trigger from the task review skill.
- Leave no blocking point open. Remaining points must be optional suggestions or nits, and the approval must say they do not hold up merging.
- Judge behavior, not wording. An outdated PR description, document or code comment is at most a nit, never a reason to hold.
- Leave reasonable design choices to the author. If the author has explained a choice and it has no concrete blocking defect, trust it. Do not hold approval for the reviewer's preferred version.
- Weigh a proposed fix against the harm it could cause. A rare problem may remain when the fix would break more valuable behavior or cause greater harm. Explain the accepted trade-off; “rarely twice” can beat “rarely never.”
- For dependent PRs, check them together and, when approval is authorized and each is ready, approve both. Leave merge order to the author; do not post it in comments or approvals. Keep dependency context in the tracker. A dependency alone is not a reason to hold approval.

## Hold

Hold when a required fix has not arrived, an answer needed to judge correctness is missing, a decision only the user can make remains open, new commits have not been checked, or the PR has not been reviewed at all. Name what remains and who owns the next action. An optional question does not hold approval.

Mark suggestions and nits as optional from the start. If a requested fix or question is settled by the author's explanation or an accepted trade-off, record that outcome so it does not remain a false blocker. Do not count a reasonable, explained design choice as an unanswered decision.

## Comments and follow-up

Each comment must stand alone. Name the issue in plain words and link to the code; never refer to private numbering such as “finding 4.” Follow the task review skill's rules for context, evidence and possible fixes.

Usually present the concern as a question to the author. Ask for clarification when needed, and make the reviewer's recommendation clear. Keep questions marked by weight so an optional suggestion cannot be mistaken for a required fix.

Mark each point **must fix**, **suggestion**, or **nit (no action needed)**. Check existing comments, review threads and bot reviews before posting. Do not repeat a point already raised; add to its thread only when there is new evidence or a useful clarification.

Post each point once in the repository that owns the change. Use separate inline threads for findings tied to code. Keep internal ratings, flags, team summaries and agent details in the tracker. With nothing left for the author to act on, submit the approval with an empty body. Keep validation notes (tests run, commits checked, counts) in the tracker; mention them in a comment only as evidence for a specific finding.

After checking a fix, list only what remains. If nothing remains, approve with an empty body; do not post a separate acknowledgement. Keep resolved points in the tracker without presenting them as open. An approval with only optional points left should say so.
