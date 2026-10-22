---
name: pr-task-review
description: "Review related PRs as one task with senior-code-reviewer context followed by paranoid-bunch. Check findings, compare risk, and flag human decisions. Leave code unchanged; post issues and possible fixes as authorized PR comments."
---

# PR task review

If the user supplies run conditions, read [conditions rules](../pr-review-flow/references/conditions.md) and carry the effective scope through this stage. Keep run-specific filters outside the skill.

Read [shared state rules](../pr-review-flow/references/tracker.md) and the installed $paranoid-bunch skill; in this bundle it is [paranoid-bunch](../paranoid-bunch/SKILL.md). If unavailable, report the missing dependency rather than claim its review was run.

Read [approval and hold rules](../pr-review-flow/references/approval.md). Apply them to findings, follow-up and readiness for approval, and carry them into every delegated task.

Use one task with all related PRs, initial labels/risk, prior decisions and current heads. For a requested batch, delegate one task per subagent within available slots. Choose a review depth per task before delegating, record it with a one-line reason in the tracker, and run the passes that depth names plus the new UI check below when it applies.

## New UI check

Check the diff against the base for added user-facing UI: a new screen, section, control or flow. Updates to existing UI alone do not trigger this check. A new source file, moved component or refactor does not by itself mean new UI. For mixed changes, review the additions in the context of the existing app.

When the PR adds UI, call [$design-roast](../design-roast/SKILL.md) at any review depth. Supply the added UI, checked commit, task goals, user needs and available brand and design evidence. Record whether the check ran, was skipped or lacked evidence in the tracker. On later pushes, revisit only affected findings or additions.

Check the roast's findings before posting. Comment only when the addition is below the app's standard and would make it worse: for example, it hinders the task, misses a business need or breaks a useful design pattern. For marginal improvements, weigh the benefit against the author's time; usually say nothing. Do not post taste-based nits or a roast summary when no useful finding remains.

Every design comment must include an image showing the concern or proposed improvement, with useful alt text. An AI-generated concept is enough; label it as such. Use an attachment or image URL the PR's readers can access, never a local path or an invented upload URL. If an image cannot be generated or attached, keep the design comment as a draft and report the limit. Other code findings can proceed. Apply the normal posting, duplicate and approval rules below; missing artwork alone is not a code defect.

## Review depth

Pick the lowest depth that fits; any one trigger for a higher depth moves the task up.

- **Light** — the lead agent reads the diff and runs existing checks; no subagents, no proof scripts. Use for low initial risk and changes such as docs, config values, renames, dependency bumps with passing CI, test-only changes, or small logic changes covered by tests in the same PR. Also use for a delta review of a small push that only answers earlier comments or merges the base branch; follow the quick re-approval rule in the approval rules.
- **Standard** — one senior-code-reviewer pass, then one paranoid-bunch call. paranoid-bunch already runs its own two adversarial reviews; never call it twice. Use for normal feature or fix work in one repository with tests.
- **Deep** — the standard passes plus proof scripts where useful and data tracing across repositories. Use only when the task touches money, auth or permissions, data migrations or deletes, shared contracts across repositories, concurrency, or when a lighter pass found a must-fix issue it could not confirm.

Stop digging once the evidence settles each point: do not write proof scripts for a finding that the source already shows, and do not search for more issues in a PR whose depth is light. Raise the depth mid-review only with a concrete reason, and note it in the tracker. If an agent, model or delegation support is unavailable, report the missing pass and reduced review coverage; do not claim it ran.

Every review agent must leave the target code, tests and configuration unchanged. Do not apply fixes, refactor, add tests to the target or run tools that rewrite its files. Run existing checks without repair steps; put any separate proof scripts and output under `tmp/`. If a check needs target edits, report that limit. A clear fix, failed test or accepted finding does not authorize implementation; that needs a separate user request.

Include this handoff in every delegated review or check, including nested subagents: “Review only. Leave the target code, tests and configuration unchanged. Return issues with evidence, locations and possible fixes, marked must fix, suggestion, or nit (no action needed). Apply the supplied approval and hold rules. Do not nitpick PR descriptions; outdated docs or comments are optional minor notes, not findings. Write notes or proof scripts only in assigned scratch space. Do not post to GitHub; the lead agent checks and posts findings.”

At standard or deep depth, before calling paranoid-bunch, call the installed senior-code-reviewer subagent on the same task and pinned commits. Give it the related PRs, scope, prior decisions and the handoff above. Ask it to return findings only as supporting material, with evidence, code locations and possible fixes. Any verdict in its output is advice, not a GitHub action. It must not apply findings, edit the target or post comments.

Wait for that pass to finish, then keep its output in task-specific scratch under `tmp/` or in the task's context. Pass that output to paranoid-bunch as provisional supporting material, not as conclusions it must adopt. It must inspect source and tests, challenge those points and look for other issues. The lead agent checks its blended result before any posting. Do not publish the senior review directly or save it in permanent repository documents; its notes remain temporary and tied to the checked commits. After a push, refresh supporting material for the affected changes before using it in a delta review.

Trace the data across repositories. Check correctness and meaningful test coverage alongside too much scope, needless layers, reinvented tools, unneeded compatibility and excessive handling of unlikely cases. Pin reviewed heads. For a pending review that changes, inspect the delta and rerun only affected checks.

Blend duplicate findings and preserve real disagreement. Challenge each issue against source and tests. Record a concrete trigger, effect, location/owning PR, evidence, practical risk and smallest useful fix. A forced test failure proves possibility, not likelihood; distinguish normal input from artificial input and do not inflate a minor suggestion into a blocker. Then apply the real-world value filter in the approval rules to every checked finding.

Do not nitpick PR descriptions for wording, format or completeness. Be lenient with outdated documentation and code comments. If worth mentioning, keep them brief and label them as optional minor notes, separate from findings. They do not count as issues, raise risk, block review or need a human decision. Judge code behavior from source and tests rather than treating a mismatch with prose as proof of a defect.

Keep findings and evidence in scratch under `tmp/`, never `docs/review.md`. This workflow's explicit temporary-notes choice overrides that destination in paranoid-bunch. Preserve accepted, deferred and rejected decisions; do not reopen them without new evidence that changes their basis.

Requesting a task review authorizes the lead agent to post checked points on the user's behalf within the named PRs or batch, unless the user asks for a read-only review or drafts. Post points that need no user decision automatically; do not wait for the walkthrough or ask for permission again. Never post dropped findings or long-tail risks. Post only feedback the author can act on. Use a separate inline review thread for each finding tied to code, so the author can resolve it. Use a top-level comment only for a point with no suitable code location. Check existing comments, review threads and bot reviews first. Post each issue or shared decision once, on the PR in the repository that owns the change; link from other PRs only when needed. Save returned links. After checking fixes, list only what remains; if nothing remains, approve with an empty body. Keep optional notes brief and useful; automatic posting is not a reason to add nits. Route decisions needing the user's eyes to $pr-walkthrough without delaying other comments. For a read-only review, return drafts instead.

Every posted comment must stand alone without repeating background. Name the problem in plain words, link to the code and give enough evidence and context to understand its effect and a possible fix. Do not refer to private notes, local paths or private finding numbers such as “finding 4”; useful shared ticket keys are welcome. Link to shared decisions instead of retelling them on each PR. Before posting, read each comment without the review notes and remove text that does not help the author act.

Usually frame a comment as a question to the author: explain the concern, then ask for clarification or whether a possible fix would address it. Ask when something is unclear rather than guessing. Mark every point **must fix**, **suggestion**, or **nit (no action needed)**; a question must not hide a requirement. State the lead reviewer's checked recommendation clearly instead of forwarding an agent's priority or verdict. Do not invent the user's position on an open decision.

Keep validation notes (tests run, commits checked, counts) in the tracker. Mention validation in a comment only as evidence for a specific finding. With nothing left for the author to act on, post nothing; when approval is authorized and the checked commit is ready, approve with an empty body.

Reassess risk after review. In the tracker, preserve initial risk and flag **RISK CHANGED: old → new** with one evidence-based reason, including decreases. Record **HUMAN REVIEW** there when too much scope, reinvented tools, architecture, product trade-offs or issues with no clear fix leave a decision only the user can make. A reasonable design choice explained by the author does not need this flag merely because the reviewer prefers another approach. State the exact decision and PR; human judgment is separate from risk. Keep risk ratings, review flags and the team story out of PR comments.

After checking the findings, approve ready PRs automatically on the user's behalf under the approval and hold rules. A task review authorizes this within its scope unless the user requests read-only work, drafts or no approvals. The lead agent verifies the current head and submits approval tied to the checked commit; do not wait for confirmation or for other PRs with open decisions. Save the review link and approved commit in the tracker.

Bring user-owned decisions to the user in the walkthrough, not by mentioning their own handle on the PR. Use a formal review request only when authorized, and record its result in the tracker without a separate announcement comment. A review run does not authorize changes-requested verdicts, code changes, commits, pushes, merges or new PRs.

Return checked findings, verification limits, changed risks, the human queue and the long-tail risks for the user to review. Add one paragraph, readable in under a minute, explaining team changes and what the user should learn. End with a list of links to every comment, review thread, approval and review request posted in this run, one per line with its PR and a few words on what it says; write “none posted” when empty. Pass that queue to $pr-walkthrough if discussion is requested.
