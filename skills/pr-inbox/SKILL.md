---
name: pr-inbox
description: "Sort GitHub notifications, dismiss handled threads, and group unread plus known pending PRs by task. Use for inbox cleanup or listing pending reviews; does not review code."
---

# PR inbox

If the user supplies run conditions, read [conditions rules](../pr-review-flow/references/conditions.md) and carry the effective scope through this stage. Keep run-specific filters outside the skill.

Read [shared state rules](../pr-review-flow/references/tracker.md). These five skills are shipped together; if that reference is missing, locate the installed pr-review-flow skill before continuing.

1. Resolve the user's GitHub account and requested repository scope. Carry forward session filters; do not hardcode an organisation or exclude dependencies without a stated preference.
2. Fetch every page of unread notifications. Add read items with a known pending action from the tracker, the inbox, or the user. `all=true` is candidate discovery, not proof of pending state.
3. Read relevant activity and the user's own reviews. Classify each item: review request, follow-up, feedback on the user's PR, author-owned fix, approval/merge, or bot/check notice. Notification `reason` is a subscription reason, not a reliable description of the latest event.
4. Give one line per item: what it is and the recommended action. Keep feedback on the user's own PRs separate from requests for their review.
5. When cleanup is authorized, mark approved-by-user, merged/closed, waiting-with-no-user-action, and obvious no-action items done. Do not ask again for those routine choices. Preserve pending actions and return unread plus known read pending, grouped by ticket/task across repositories.

“Discard” means mark done: `DELETE /notifications/threads/{id}`. `PATCH` only marks read. Record successful actions and failures separately. New activity on an approved PR does not reopen review by default. Clearing a batch of the user's own PRs does not authorize clearing future feedback.

Deduplicate PRs by repository and number. Show unread/read-pending status and count PRs separately from notification threads. Do not claim the UI inbox count from historical API results. If the user asks only to list or classify, do not mutate notifications.

Pass the resulting task groups and tracker to $pr-triage when assessment is requested.
