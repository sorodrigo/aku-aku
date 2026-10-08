---
name: pr-walkthrough
description: "Explain flagged PRs in plain short stories, discuss human decisions one at a time, and track review progress. Use for guided review after findings or human-review flags exist."
---

# PR walkthrough

If the user supplies run conditions, read [conditions rules](../pr-review-flow/references/conditions.md) and carry the effective scope through this stage. Keep run-specific filters outside the skill.

Read [shared state rules](../pr-review-flow/references/tracker.md). Start from the task's findings, human-review queue and prior decisions. This is guided discussion, not another full review; use $pr-task-review if a new review is needed.

Present the team story once in a short paragraph readable in under a minute: what is changing, how the parts connect and what the user needs to learn. Then take one task or PR at a time.

Before its first decision, explain the task in at most four short lines. For each issue, state “Issue N of M,” what happens, what goes wrong, and what the proposal changes. Say what needs the user's eyes and why. Use plain familiar words and avoid jargon.

Record each decision as accepted, deferred, rejected or an open question. For an open question, get the user's recommendation rather than invent it. Carry decisions forward and keep review notes temporary. Answer questions briefly, then continue the current walkthrough.

Accepting a finding records agreement with the review; it does not authorize a code change. Share the issue and possible fix as a PR comment when posting is authorized. Implementation needs a separate user request.

“Next” moves the discussion forward; it does not mean approval, agreement or that a pending action is complete. If the user says they approved a PR elsewhere, record that and check GitHub when needed; do not let an old tracker override their correction. Submit approval only when instructed, then update the tracker and discard its notification when inbox cleanup is authorized.

Return the recorded decisions and remaining human actions. Never silently resume code changes, batch reviews or publishing while the user is walking through PRs.
