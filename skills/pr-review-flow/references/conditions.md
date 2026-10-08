# Run conditions

A run may take a conditions file supplied by the user. Keep team names, organisations, repository lists and other run-specific choices outside the skills. Do not search for and load unrelated conditions files automatically.

Use a plain Markdown file. It can state review scope, inbox cleanup scope, exclusions, task filters and output preferences in ordinary language. Read the file once and carry the resolved conditions into every stage and delegated task. The user's current instructions override the file; the file overrides earlier scope defaults. Conditions narrow or shape the work; they do not grant authority to post, approve or perform other external actions.

Record the file path, effective conditions and any resolved membership snapshot/check time in the scratch tracker. Reapply review filters to candidates from both notifications and older pending entries. A condition on reviewing PRs does not authorize dismissing excluded notifications. Use the same boundary for triage, reviews, comments and human-review requests unless the user explicitly gives different scopes.

## Team author filter

“Only review PRs from members of TEAM” means the PR's author must be a current member of that GitHub team, not that the team owns the repository or was asked to review it.

Resolve an explicit team URL to its organisation and team slug. Fetch every page of current members, for example `GET /orgs/{org}/teams/{team_slug}/members`. Compare GitHub logins without case sensitivity against each PR's author. Resolve membership once per run and pass that snapshot to subagents. Do not use an old hardcoded member list.

If membership cannot be read, report the failed condition and do not broaden the review scope or treat unknown authors as eligible. Continue independent work whose scope can be established. An empty confirmed member list yields no eligible authors; an access error is not an empty list.

Group eligible PRs by task after applying filters. Linked PRs outside scope may be read for context, but do not add them to the review queue, publish findings on them or request their review without authorization. Count eligible PRs and excluded/unknown candidates separately when useful.
