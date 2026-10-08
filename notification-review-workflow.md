# Notification and PR review workflow

Use the full flow or call one stage. The skills share a scratch tracker, so pending work and settled decisions carry forward.

| Skill | Use for | Result |
|---|---|---|
| [$pr-inbox](skills/pr-inbox/SKILL.md) | Classify notifications and clean the inbox. | Unread plus known read pending PRs, grouped by task. |
| [$pr-triage](skills/pr-triage/SKILL.md) | Add shallow labels and initial risk. | Task review queue with reasons. |
| [$pr-task-review](skills/pr-task-review/SKILL.md) | Get senior-code-reviewer context, then review each task with paranoid-bunch. | Checked findings, authorized PR comments, risk changes and human flags. |
| [$pr-walkthrough](skills/pr-walkthrough/SKILL.md) | Discuss flagged decisions one at a time. | Recorded decisions and remaining actions. |
| [$pr-review-flow](skills/pr-review-flow/SKILL.md) | Connect the stages. | The full workflow, resuming from current progress. |

Install the five skills together. Task review also uses [paranoid-bunch](skills/paranoid-bunch/SKILL.md). [Shared state rules](skills/pr-review-flow/references/tracker.md) define the tracker, counts and action limits.

Set up the [senior-code-reviewer subagent](subagents.md) too. It runs before paranoid-bunch and returns findings as temporary supporting context. It makes no fixes or GitHub posts. Paranoid-bunch checks that context alongside its own review; the lead agent checks the combined findings before posting.

For example: “Use $pr-inbox to clean my GitHub inbox,” or “Use $pr-review-flow to group pending work, classify risk, review each task and post findings on my behalf.”

Read, done, reviewed and approved are separate states. Review notes stay in scratch; user decisions carry forward. Record approval only after submission succeeds. A review run does not authorize merges or code changes.

Calling task review or the full review flow posts checked points and approves ready PRs automatically on your behalf, within the review scope. It does not wait for confirmation or the walkthrough. Approval needs a checked commit, enough evidence for confidence and no open blocking points or required decisions. Ask for read-only review or drafts to prevent comments and approvals, or no approvals to keep them pending. Decisions needing your eyes stay in the human queue.

Review agents leave code, tests and configuration unchanged, including when checking a finding. They return issues and possible fixes; the lead agent checks them and posts PR comments on your behalf when authorized. Accepting a finding does not start a fix. Ask separately for implementation.

Post only feedback the author can act on. Keep comments short and clear without relying on private review notes. Usually explain the concern as a question, marked must fix, suggestion, or nit (no action needed). Use separate inline threads for code findings. Post each point once on the PR in the repository that owns the change; link from other PRs only when needed. Keep internal ratings, flags and team summaries in the tracker. With no findings, submit an authorized approval without an extra summary comment. Keep check results brief, usually one line with the checked commit and any limits.

Do not nitpick PR descriptions. Outdated docs and code comments may be mentioned as brief, optional minor notes; they do not count as review issues or raise risk.

Use the [approval and hold rules](skills/pr-review-flow/references/approval.md): approve the exact checked commit only when no blocking point remains, trust reasonable choices explained by the author, and weigh the harm of proposed fixes. Leave merge order to the author; do not post it in comments or approvals. New pushes need a delta review. Mark comments must fix, suggestion, or nit (no action needed); check earlier comments and bot reviews, acknowledge fixes briefly, and list only what remains.

## Run with conditions

Keep run-specific choices in a separate Markdown file. Keep project conditions in the project workspace, separate from this skill repository. For example, a Payments conditions file can limit reviews to PRs authored by current team members.

```text
Use $pr-review-flow with conditions from /path/to/project/review-conditions/payments.md.
```

Any stage can take the same file. Current instructions can override its conditions. See [conditions rules](skills/pr-review-flow/references/conditions.md) for membership checks and scope boundaries.
