---
name: pr-task-review
description: "Review related PRs as one task using paranoid-bunch, validate findings, compare risk, and flag human decisions. Post findings or request reviews only within the user-authorized scope."
---

# PR task review

If the user supplies run conditions, read [conditions rules](../pr-review-flow/references/conditions.md) and carry the effective scope through this stage. Keep run-specific filters outside the skill.

Read [shared state rules](../pr-review-flow/references/tracker.md) and the installed $paranoid-bunch skill; in this bundle it is [paranoid-bunch](../paranoid-bunch/SKILL.md). If unavailable, report the missing dependency rather than claim its review was run.

Use one task with all related PRs, initial labels/risk, prior decisions and current heads. For a requested batch, delegate one task per subagent within available slots. Each task gets two independent adversarial passes with different models, following paranoid-bunch. If that model or delegation support is unavailable, report the reduced review coverage.

Trace the data across repositories. Check correctness and meaningful test coverage alongside too much scope, needless layers, reinvented tools, unneeded compatibility and excessive handling of unlikely cases. Pin reviewed heads. For a pending review that changes, inspect the delta and rerun only affected checks.

Blend duplicate findings and preserve real disagreement. Challenge each issue against source and tests. Record a concrete trigger, effect, location/owning PR, evidence, practical risk and smallest useful fix. A forced test failure proves possibility, not likelihood; distinguish normal input from artificial input and do not inflate a minor suggestion into a blocker.

Keep findings and evidence in scratch under `tmp/`, never `docs/review.md`. This workflow's explicit temporary-notes choice overrides that destination in paranoid-bunch. Preserve accepted, deferred and rejected decisions; do not reopen them without new evidence that changes their basis.

When the user authorizes posting on their behalf, that covers the named batch: post checked findings on the PR owning the fix, inline when useful. Check existing comments, avoid duplicates, link companion test PRs, distinguish required fixes from suggestions, and save returned links. For clear fixes, this requested posting mode replaces the skill's interview. Without posting authorization, prepare findings for $pr-walkthrough. Do not invent a user recommendation for an unresolved design question.

Reassess risk after review. Preserve initial risk and flag **RISK CHANGED: old → new** with one evidence-based reason, including decreases. Flag **HUMAN REVIEW** for too much scope, reinventing tools, architecture, product trade-offs or issues with no clear fix. State the exact decision and PR; human judgment is separate from risk.

When posting is authorized, mention the user's actual GitHub handle and ask for the flagged decision on its PR. Use a formal review request when authorized, and record its result. Do not count an internal flag as a delivered request. Posting findings alone does not authorize approvals, changes-requested verdicts, commits, pushes, merges or new PRs.

Return checked findings, verification limits, comment/request links, changed risks and the human queue. Add one paragraph, readable in under a minute, explaining team changes and what the user should learn. Pass that queue to $pr-walkthrough if discussion is requested.
