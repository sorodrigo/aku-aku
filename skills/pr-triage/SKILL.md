---
name: pr-triage
description: "Add shallow change labels and initial risk to pending PRs grouped by task. Use to sort a review queue before detailed review; does not publish GitHub labels or comments."
---

# PR triage

If the user supplies run conditions, read [conditions rules](../pr-review-flow/references/conditions.md) and carry the effective scope through this stage. Keep run-specific filters outside the skill.

Read [shared state rules](../pr-review-flow/references/tracker.md). Use supplied groups or an existing tracker; run $pr-inbox only if collecting inbox work is part of the request.

Read titles, descriptions, changed-file scope and linked plans as needed. Keep this pass shallow. Remove tasks where the user has already shared review from the fresh-review queue; record reviewed rather than treating them as approved. Retain named follow-ups and user-requested rechecks.

Use overlapping labels in the tracker:

| Label | Meaning |
|---|---|
| Maintenance | Upkeep, cleanup, tools and tests that preserve intended behavior. |
| New feature | A new ability or user flow. |
| Bug fix | Corrects wrong behavior. |
| Architectural | Changes boundaries, ownership, shared design or data flow. |

Assign low, medium or high initial risk and one reason. Low is narrow and easy to undo; medium changes behavior or a service boundary with limited reach; high has plausible serious effects on money, accounting, data, access or hard-to-reverse changes. Judge likely inputs, impact, reach and recovery together. Mark uncertainty; do not treat missing evidence as low risk.

Use the highest supported risk for a task, with per-PR differences where useful. This is impact risk, not finding priority. Mark initial risk provisional and preserve it when later review changes the assessment. Do not write GitHub labels unless asked.

Return a compact task table: PR links, labels, initial risk/reason, and next action. Pass unreviewed tasks to $pr-task-review when detailed review is requested.
