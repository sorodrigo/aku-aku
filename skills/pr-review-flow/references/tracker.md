# Shared review state

Keep one scratch tracker for all stages, under `tmp/` in the chosen workspace. Use the existing tracker when available. Track only fields needed for the work, and keep notes out of permanent repository review documents. Install the five PR skills together with `paranoid-bunch` so they can read this shared reference, call one another and run task reviews.

## Run scope

When a conditions file is supplied, use [conditions rules](conditions.md). Save its path, effective filters and resolved membership evidence alongside the tracker.

## States

| State | Meaning |
|---|---|
| Unread/read | Notification reading state; read can still need action. |
| Done | Notification dismissed; not a PR verdict. |
| Pending | A named action and owner remain, supported by the user, tracker or current inbox. |
| Review shared | The user has given feedback; not approval. |
| Approved | The user's approval tied to its checked commit; another reviewer's approval does not count. |
| Open/merged/closed | PR state, independent of pending work and notifications. |

Distinguish the user (inbox owner and human decision maker), the assistant and each PR author. Resolve the user's current account; never hardcode a person's name or GitHub handle.

## Tracker fields

- Identity: task/ticket, repository, PR link/number, PR author, notification IDs.
- Evidence: current head, reviewed head, approved commit/review link, latest activity, status sources and check times.
- Assessment: overlapping labels, initial risk/reason, reviewed risk/reason, findings and comment links.
- Progress: review shared, own approval, pending action/owner, human flag/decision, accepted/deferred/rejected/open-question status, merge order and linked dependencies.
- Inbox: unread/read, discard request, done action result, separate inbox-state evidence where available.

Update only the state justified by evidence. Tool success proves the action was accepted, not every downstream or UI state. A failed or uncertain action remains reported as such. A user correction takes precedence over stale notes; verify live state where useful.

Use [approval and hold rules](approval.md) to track readiness. Preserve earlier approval as history when the head changes, but mark the new commit as needing a delta review. Do not carry approval forward to unchecked commits or dismiss their review work as already handled.

## Candidate discovery and counts

Fetch all pages. Unread-only queries omit read pending work; `all=true` can return historical or done threads. Neither alone defines the pending queue. A notification subscription reason is not the latest event. Do not promote every returned open PR to pending.

Pending work is unread items within scope plus explicitly known read pending items, minus authorized dismissals. Refresh live activity and retain a record of successful done actions; later activity can make a thread unread again. Reapply the authorized cleanup rule for that new event rather than claim a prior action failed. If UI state cannot be confirmed, label the source/count precisely.

Deduplicate PRs by repository and number, tasks by ticket/shared outcome, and notification threads by ID. Count each separately. Preserve read pending actions even when unread-only queries no longer show them. Clearing a waiting notification can retain a follow-up note without adding it to the user's active-action queue.

## Authority

Review agents leave target code, tests and configuration unchanged, including during checks. Only scratch notes and separate proof scripts may be written. Carry this limit into all subagent handoffs. Accepted findings are review decisions, not permission to apply fixes; implementation needs a separate user request.

Inbox listing and triage are read-only. Apply cleanup rules when cleanup is requested or already authorized. A task review or full review flow authorizes automatic comments for checked points that need no user decision within the named PRs/batch, unless the user requests read-only work or drafts. Do not ask again or delay those comments for the human queue. A one-time discard of authored-PR feedback is not a rule to hide future feedback.

Approval, a changes-requested verdict, commits, pushes, merges and new PRs need their own user instruction. Never treat “reviewed,” “next” or “all good for now” as a submitted GitHub approval. Do not change GitHub labels merely because the tracker has labels.
