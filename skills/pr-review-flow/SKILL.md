---
name: pr-review-flow
description: "Run the full GitHub inbox-to-review workflow: cleanup, task grouping, labels and risk, task reviews, and human walkthrough. Use when the user requests the full review flow rather than one stage."
---

# PR review flow

If the user supplies run conditions, read [conditions rules](references/conditions.md) and carry the effective scope through this stage. Keep run-specific filters outside the skill.

Read [shared state rules](references/tracker.md). This skill connects the four stages; each may also run alone. Locate the named installed skill before invoking it; bundled entrypoints are linked below. Do not copy its rules or silently substitute a missing skill.

Review leaves the target code, tests and configuration unchanged. Neither the lead agent nor subagents may apply fixes, including during checks. Put issues and possible fixes in PR comments on the user's behalf when posting is authorized. Carry this limit into every delegated task. A clear fix or an accepted finding is not permission to implement it; that needs a separate user request.

1. Use [$pr-inbox](../pr-inbox/SKILL.md) to classify notifications, perform authorized cleanup and collect unread plus known pending tasks.
2. Use [$pr-triage](../pr-triage/SKILL.md) for shallow overlapping labels and initial risk. Exclude already-reviewed work from fresh review while keeping clear follow-ups.
3. Use [$pr-task-review](../pr-task-review/SKILL.md) on unreviewed tasks, including related PRs across repositories. It runs paranoid-bunch, checks findings, compares risk and flags human decisions.
4. Use [$pr-walkthrough](../pr-walkthrough/SKILL.md) for flagged decisions, one task at a time, when the user wants to discuss them.

Start at the stage the user needs and reuse the scratch tracker. Do not repeat settled steps, reopen approved PRs from notification activity alone, or run the full flow when asked only to clean or list an inbox. A push after the checked commit does need a fresh delta review; follow [approval and hold rules](references/approval.md).

A full-flow invocation authorizes analysis, the delegation specified by the review skill, and automatic posting of checked points that need no user decision within the review scope. A request for read-only review or drafts overrides posting. Do not wait for the walkthrough or ask again before posting those comments. Infer cleanup and formal review-request authority from the actual request and prior session instructions. Approval, a changes-requested verdict and implementation still need their own user instruction.

Hand off a short team story, risk changes, human decisions and pending owners. Report only verified actions; distinguish code review completion, user approval and inbox cleanup.
