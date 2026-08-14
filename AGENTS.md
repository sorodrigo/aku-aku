# TDD

Test coverage and TDD are explicitly valued ("in this ship we care about test coverage and TDD"). For any feature or fix, write tests before or alongside the code — never ship implementation without them. Prefer unit tests for pure logic and service boundaries that can be isolated. Use integration tests when behavior depends on database state, ORM mappings, request flow, or cross-service persistence. Do not insert test data directly when the project's test-data tooling can express the state — keep test setup deterministic and reusable.

The RED step must fail on *behavior*, not on a missing module: when introducing a new function, first create a stub that mirrors the current (broken) behavior so the test runs production code and fails on its assertions — a "Cannot find module" error (or any import/compile error) does not count as a valid failing test.

# Testing principles (Kent C. Dodds)

Follow these in all projects:

- **The more your tests resemble the way your software is used, the more confidence they can give you.** Test from the perspective of the code's actual users — end users interacting with output and developers calling the public API — not internals.
- **Test use cases, not implementation details.** Implementation details are things users of the code will not use, see, or know about (internal state, private methods, lifecycle hooks). Testing them causes false negatives (tests break on refactors that don't change behavior) and false positives (tests pass while real bugs ship).
- **Write tests. Not too many. Mostly integration.** Integration tests give the best confidence-per-effort. Use unit tests for complex pure logic, E2E for critical happy paths, static analysis (types, linting) as the foundation. Don't chase 100% code coverage — returns diminish past ~70%; aim for use-case coverage instead.
- **Avoid over-mocking.** Mocking removes all confidence in the integration between the code under test and what's mocked. Mock only what's unavoidable (network, third parties, clocks).
- **Respect system boundaries.** Test the behavior our code owns at a third-party boundary: the request or arguments we send, how we handle documented responses and failures, and the user-visible result. Do not test that a framework, cloud service, SDK, browser, database engine, or other dependency correctly implements its own contract. Use the smallest boundary fake needed for our tests, and rely on that dependency's tests and documentation for its internals. Add a real integration or smoke test only when it verifies our configuration or compatibility with the service, not the service's internal behavior.
- **Prioritize by impact.** Ask "what would be the worst thing to break?" — cover those flows first with happy-path integration/E2E tests, then edge cases, then unit tests for tricky logic.
- **To know what to test:** identify who the users are, write down how you'd manually verify the feature, then automate those instructions.
- **Do not write a test whose only purpose is to assert the edit you just made.** A test that restates a content, markup, or styling change — "the marker is no longer the character `●`", "this control renders an `<svg>` rather than text", "the copy now reads X" — documents a diff, not a use case. It cannot fail for any reason other than someone deliberately changing that decision back, so it buys no confidence and turns a later redesign into a test-fixing chore. The same goes for commentary explaining what the old code used to be; that belongs in the change description, not in the suite. When a change is purely presentational, the honest amount of new test code is often none. Cover the behavior around it instead (the control still advances the slideshow), or leave it to types and review.

Sources: kentcdodds.com — write-tests, testing-implementation-details, how-to-know-what-to-test, the-testing-trophy-and-testing-classifications.

# Code style

## Avoid nested ternaries

Never use nested ternary expressions. Sacrifice terse code in favor of readability. Write code as though it is harder to read than to write: future editors will not know all of your intentions or context, so make the control flow easy to follow with explicit conditionals, early returns, or well-named intermediate values.

# Writing

Treat every word and sentence as starting with negative points because it costs the reader attention. Keep it only when it has a clear purpose and the value it delivers more than offsets that cost; if its net value is not positive, remove it. A stated purpose is not enough: text that fails to achieve its purpose adds no value. Prefer plain, familiar language. Uncommon or ornate words carry a higher cost and must add proportionally more precision or meaning to earn their place.

# Comments

Keep comments to the absolute minimum. A comment is a flag raised over something strange and important — a non-obvious constraint, a surprising ordering requirement, a workaround for someone else's bug, a trap the next reader will otherwise fall into. Drowning those few real flags in a sea of narration is how they stop being read. Code that says what it does needs no comment saying the same thing again; if a comment is needed to explain *what* is happening, prefer renaming or restructuring until it isn't.

Never comment on what changed. Comments describe the present and warn about the future — never the past. Specifically, never write:

- **What the code used to be** — "previously this used X", "replaced the old Y", "this no longer does Z", "was hardcoded before".
- **That something was recently added, fixed, or moved** — "new", "updated", "moved here from …", "TODO: remove after migration" where the migration is already done.
- **Narration of your own edit** — a comment whose only reader is the person reviewing this diff. That belongs in the commit message or the PR description, not in the file.
- **Aspirational or stale framing** — "will become X", "stands in until Y exists", "for now" — once X or Y has landed. When you change code that carries such a comment, delete or rewrite the comment in the same edit; a comment describing a state that no longer holds is worse than no comment.

Treat an outdated comment you happen to read as a defect to fix, not context to preserve.

# Git

Do not create git commits on your own. Only commit when I explicitly ask you to. This holds even mid-task and even when a plan, skill, or workflow (e.g. subagent-driven development) suggests frequent commits — make the file changes and leave them for me to review and commit. The same applies to merging, pushing, and opening PRs: never do them unless I ask.

# Boolean function arguments

Treat a boolean argument that switches a function between different behaviors as a
code smell. It often means the function has more than one responsibility, obscures the
call site, and leaves no room for a third mode. Prefer separate cohesive functions or
an explicit enum/string union. A boolean is fine when the function's single purpose
makes both states unambiguous, such as `setLoading(true)`. When a flag is justified
among other arguments, put it in a named options object so its meaning is visible.

Reference: https://alexkondov.com/should-you-pass-boolean-to-functions/
