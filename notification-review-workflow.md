# Notification and PR review workflow

Use the full flow or call one stage. The skills share a scratch tracker, so pending work and settled decisions carry forward.

| Skill | Use for | Result |
|---|---|---|
| [$pr-inbox](skills/pr-inbox/SKILL.md) | Classify notifications and clean the inbox. | Unread plus known read pending PRs, grouped by task. |
| [$pr-triage](skills/pr-triage/SKILL.md) | Add shallow labels and initial risk. | Task review queue with reasons. |
| [$pr-task-review](skills/pr-task-review/SKILL.md) | Review each task with paranoid-bunch. | Checked findings, authorized PR comments, risk changes and human flags. |
| [$pr-walkthrough](skills/pr-walkthrough/SKILL.md) | Discuss flagged decisions one at a time. | Recorded decisions and remaining actions. |
| [$pr-review-flow](skills/pr-review-flow/SKILL.md) | Connect the stages. | The full workflow, resuming from current progress. |

Install the five skills together. Task review also uses [paranoid-bunch](skills/paranoid-bunch/SKILL.md). [Shared state rules](skills/pr-review-flow/references/tracker.md) define the tracker, counts and action limits.

For example: “Use $pr-inbox to clean my GitHub inbox,” or “Use $pr-review-flow to group pending work, classify risk, review each task and post findings on my behalf.”

Read, done, reviewed and approved are separate states. Review notes stay in scratch; user decisions carry forward. Creating or invoking a skill does not approve PRs, merge changes or grant every posting action.

Review agents leave code, tests and configuration unchanged, including when checking a finding. They return issues and possible fixes; the lead agent checks them and posts PR comments on your behalf when authorized. Accepting a finding does not start a fix. Ask separately for implementation.

Each PR comment stands alone: explain the affected flow, trigger, actual and expected results, practical effect, evidence and possible fix. State any limits to the checks. Explain related PRs and decision choices in the comment; readers should not need our private review notes.

Do not nitpick PR descriptions. Outdated docs and code comments may be mentioned as brief, optional minor notes; they do not count as review issues or raise risk.

## Run with conditions

Keep run-specific choices in a separate Markdown file. Keep project conditions in the project workspace, separate from this skill repository. For example, a Payments conditions file can limit reviews to PRs authored by current team members.

```text
Use $pr-review-flow with conditions from /path/to/project/review-conditions/payments.md.
```

Any stage can take the same file. Current instructions can override its conditions. See [conditions rules](skills/pr-review-flow/references/conditions.md) for membership checks and scope boundaries.
