---
name: pr-task-review
description: "Review related PRs as one task with senior-code-reviewer context followed by paranoid-bunch. Check findings, compare risk, and flag human decisions. Leave code unchanged; post issues and possible fixes as authorized PR comments."
---

# PR task review

If the user supplies run conditions, read [conditions rules](../pr-review-flow/references/conditions.md) and carry the effective scope through this stage. Keep run-specific filters outside the skill.

Read [shared state rules](../pr-review-flow/references/tracker.md) and the installed $paranoid-bunch skill; in this bundle it is [paranoid-bunch](../paranoid-bunch/SKILL.md). If unavailable, report the missing dependency rather than claim its review was run.

Read [approval and hold rules](../pr-review-flow/references/approval.md). Apply them to findings, follow-up and readiness for approval, and carry them into every delegated task.

Use one task with all related PRs, initial labels/risk, prior decisions and current heads. For a requested batch, delegate one task per subagent within available slots. Each task first gets a senior-code-reviewer pass, then two independent adversarial passes with different models, following paranoid-bunch. If an agent, model or delegation support is unavailable, report the missing pass and reduced review coverage; do not claim it ran.

Every review agent must leave the target code, tests and configuration unchanged. Do not apply fixes, refactor, add tests to the target or run tools that rewrite its files. Run existing checks without repair steps; put any separate proof scripts and output under `tmp/`. If a check needs target edits, report that limit. A clear fix, failed test or accepted finding does not authorize implementation; that needs a separate user request.

Include this handoff in every delegated review or check, including nested subagents: “Review only. Leave the target code, tests and configuration unchanged. Return issues with evidence, locations and possible fixes, marked must fix, suggestion, or nit (no action needed). Apply the supplied approval and hold rules. Do not nitpick PR descriptions; outdated docs or comments are optional minor notes, not findings. Write notes or proof scripts only in assigned scratch space. Do not post to GitHub; the lead agent checks and posts findings.”

Before calling paranoid-bunch, call the installed senior-code-reviewer subagent on the same task and pinned commits. Give it the related PRs, scope, prior decisions and the handoff above. Ask it to return findings only as supporting material, with evidence, code locations and possible fixes. Any verdict in its output is advice, not a GitHub action. It must not apply findings, edit the target or post comments.

Wait for that pass to finish, then keep its output in task-specific scratch under `tmp/` or in the task's context. Pass the same output to both paranoid-bunch passes as provisional supporting material, not as conclusions they must adopt. Each pass must inspect source and tests, challenge those points and look for other issues without seeing the other paranoid-bunch pass's conclusions. The lead agent checks and blends the results before any posting. Do not publish the senior review directly or save it in permanent repository documents; its notes remain temporary and tied to the checked commits. After a push, refresh supporting material for the affected changes before using it in a delta review.

Trace the data across repositories. Check correctness and meaningful test coverage alongside too much scope, needless layers, reinvented tools, unneeded compatibility and excessive handling of unlikely cases. Pin reviewed heads. For a pending review that changes, inspect the delta and rerun only affected checks.

Blend duplicate findings and preserve real disagreement. Challenge each issue against source and tests. Record a concrete trigger, effect, location/owning PR, evidence, practical risk and smallest useful fix. A forced test failure proves possibility, not likelihood; distinguish normal input from artificial input and do not inflate a minor suggestion into a blocker.

Do not nitpick PR descriptions for wording, format or completeness. Be lenient with outdated documentation and code comments. If worth mentioning, keep them brief and label them as optional minor notes, separate from findings. They do not count as issues, raise risk, block review or need a human decision. Judge code behavior from source and tests rather than treating a mismatch with prose as proof of a defect.

Keep findings and evidence in scratch under `tmp/`, never `docs/review.md`. This workflow's explicit temporary-notes choice overrides that destination in paranoid-bunch. Preserve accepted, deferred and rejected decisions; do not reopen them without new evidence that changes their basis.

Requesting a task review authorizes the lead agent to post checked points on the user's behalf within the named PRs or batch, unless the user asks for a read-only review or drafts. Post points that need no user decision automatically; do not wait for the walkthrough or ask for permission again. Put them on the PR owning the fix, inline when useful. Each comment states the trigger, effect, evidence and possible fix; suggest a test when useful. Describe fixes for the author to make. Check existing comments, review threads and bot reviews first; avoid duplicates, link companion test PRs, mark each point must fix, suggestion, or nit (no action needed), and save returned links. Acknowledge checked fixes briefly, then list only what remains. Keep optional notes brief and useful; automatic posting is not a reason to add nits. Route decisions needing the user's eyes to $pr-walkthrough without delaying other comments. For a read-only review, return drafts instead. Do not invent a user recommendation for an unresolved design question.

Every posted comment must stand alone for someone who has only the PR and its linked code. Name the affected flow and code location, explain the input or sequence that triggers the issue, and state the actual result, expected result and practical effect. Include enough source or test evidence to support the claim, say what was checked and what remains uncertain, and explain how the possible fix would help. If another PR matters, link it and explain the relevant behavior in the comment. Do not rely on private notes, local paths, internal issue numbers or “as discussed.” For a decision request, explain the options and their effects, then ask the exact question. Before posting, read each comment without the review notes and fill any gaps needed to understand or act on it.

Reassess risk after review. Preserve initial risk and flag **RISK CHANGED: old → new** with one evidence-based reason, including decreases. Flag **HUMAN REVIEW** when too much scope, reinvented tools, architecture, product trade-offs or issues with no clear fix leave a decision only the user can make. A reasonable design choice explained by the author does not need this flag merely because the reviewer prefers another approach. State the exact decision and PR; human judgment is separate from risk.

When posting is authorized, mention the user's actual GitHub handle and ask for the flagged decision on its PR. Use a formal review request when authorized, and record its result. Do not count an internal flag as a delivered request. Posting findings alone does not authorize approvals, changes-requested verdicts, commits, pushes, merges or new PRs.

Return checked findings, verification limits, comment/request links, changed risks and the human queue. Add one paragraph, readable in under a minute, explaining team changes and what the user should learn. Pass that queue to $pr-walkthrough if discussion is requested.
