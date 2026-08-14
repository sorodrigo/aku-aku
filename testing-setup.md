# Testing setup

Use this guide when introducing or rebuilding test infrastructure. It complements
the testing principles in `AGENTS.md`: that file describes what deserves a test;
this one describes the preferred machinery for running those tests.

The target shape is:

- pure logic runs in a small, fast test environment;
- server integration tests enter through the real application boundary and use a
  real disposable database;
- UI integration tests run in a real browser and mock at the HTTP boundary;
- third-party HTTP is simulated locally and unexpected network traffic fails;
- a single test command owns setup and teardown.

Most confidence should come from the two integration layers. Do not build a large
unit-test layer by mocking the pieces that those integration tests should connect.

## Defaults

### Test runner

#### Pure logic and server code

Prefer Vitest for TypeScript and JavaScript projects. Use its Node environment for
pure logic and compatible server code.

#### Browser UI

For web UI, prefer Vitest Browser Mode with the Playwright provider and Chromium
over jsdom or happy-dom.

### UI interaction

Use Testing Library for rendered UI and `user-event` or its platform equivalent for
interaction. Query the interface through roles, labels, and visible state.

### Runtime-specific exceptions

Use an environment-specific runner when it materially improves fidelity. A native
runtime adapter, a Workers pool, or Jest through Expo is better than forcing Vitest
around an incompatible runtime. Keep the same testing boundaries and principles
when the runner changes.

## Set up the database first

If production behavior depends on a database, make the real database available to
tests before designing repositories, mocks, or fixtures. ORM mappings, constraints,
transactions, migrations, and query semantics are part of the integration being
tested.

Create a test-only Docker Compose service with:

- a database and credentials that cannot be confused with development or staging;
- a separate, overridable host port;
- a health check so readiness is observed rather than guessed;
- ephemeral storage, such as `tmpfs`, unless persistence is itself under test;
- a Compose project or container name that will not collide with concurrent runs.

Wire it into the test runner's global lifecycle. One test command should:

1. start the database and wait for it to become healthy;
2. apply the application's real migrations;
3. reset it to a known state and add only essential baseline data;
4. point the application runtime at that same database;
5. tear it down after the suite, including when tests fail.

Starting the database must not require a separate manual command. Requiring Docker
itself to be running is acceptable; managing the test container is the suite's job.
Add a hard guard against using a non-test connection string.

Choose an isolation strategy deliberately. Transaction rollback is useful when all
code shares the transaction. Otherwise create uniquely identified state and clean it
through stable test-data helpers. Tests must not depend on execution order or residue
from another test. Prefer the project's factories, public APIs, or domain helpers to
scattered SQL inserts.

## Exercise the real server boundary

Server integration tests should call the same entry point a real client calls: the
HTTP application, Worker fetch handler, RPC boundary, or equivalent. Keep routing,
validation, authentication, middleware, persistence, and serialization in the path.

Use the real database and locally simulated first-party infrastructure where the
platform provides a faithful implementation. Do not replace the database, router,
or service graph with mocks merely to make the test runner convenient.

Test pure domain logic without booting the application. A recurrence calculation or
geometry function should not pay the cost of Docker; a permission or persistence flow
usually should.

## Cloudflare Workers

Cloudflare Workers need a runtime-aware test setup. Do not run Worker integration
tests in plain Node with a hand-built `env`; that can hide differences in runtime APIs,
bindings, compatibility flags, Durable Objects, and storage behavior.

Cloudflare's testing surface changes quickly. Do not copy a harness, import, or
configuration shape from this guide or from an older project. Before setting up or
changing Worker tests, consult the current official sources:

- [Workers testing overview](https://developers.cloudflare.com/workers/testing/) —
  choose the currently recommended boundary and harness;
- [Workers testing recipes](https://developers.cloudflare.com/workers/testing/vitest-integration/recipes/) —
  find maintained examples for bindings, databases, multiple Workers, outbound HTTP,
  and Workflows;
- [Workers test APIs](https://developers.cloudflare.com/workers/testing/vitest-integration/test-apis/) —
  use the current helpers instead of relying on remembered imports;
- [Workers testing known issues](https://developers.cloudflare.com/workers/testing/vitest-integration/known-issues/) —
  check runtime, storage, concurrency, timer, WebSocket, and coverage constraints.

### Apply these preferences to the current Cloudflare setup

After reading the current docs:

- keep Vitest as the runner when Cloudflare's supported setup permits it;
- run Worker behavior in the real local Workers runtime, not a generic Node facsimile;
- load the real Wrangler configuration and use local implementations of bindings;
- use fake test credentials and prevent access to production or remote resources;
- exercise the real Worker entry point for request-level integration tests;
- use Docker for an external Postgres or MySQL dependency and point both migrations
  and the Worker binding at the same disposable database;
- use the platform's maintained migration path for D1 and other local storage;
- use MSW for outbound third-party HTTP, not for first-party platform bindings;
- select the coverage provider Cloudflare currently supports and keep coverage
  report-only.

Keep host responsibilities and Worker responsibilities separate. Docker orchestration,
process execution, and host filesystem work belong in the Node side of the harness.
Bindings and Worker runtime behavior belong inside the supported Worker test context.
Use the lifecycle and isolation model documented for the chosen harness rather than
assuming ordinary Vitest behavior.

### Workflows

Treat a Workflow as durable orchestration, not as an ordinary async function. A call
to a Workflow binding only confirms that the instance was accepted; it does not mean
the Workflow's steps or side effects have completed.

Use three complementary test boundaries:

1. Test substantial domain operations directly, without wrapping them in fake
   Workflow steps.
2. Test that the owning HTTP, scheduled, queue, or event boundary hands the correct
   parameters to the Workflow.
3. Add a runtime integration test for each important Workflow path: create a real
   instance through its binding, wait for it through the Workflow introspection API,
   and assert its output or durable side effects.

Do not add a synchronous execution mode, optional callback, or alternate production
branch solely to make Workflow tests pass. That creates an execution path used by
tests but not by production.

Read the current [Workflow testing API](https://developers.cloudflare.com/workers/testing/vitest-integration/test-apis/#workflows),
[testing recipes](https://developers.cloudflare.com/workers/testing/vitest-integration/recipes/),
and [Rules of Workflows](https://developers.cloudflare.com/workflows/build/rules-of-workflows/)
before writing the harness. Use the documented introspection and control tools rather
than hardcoding API names from another project.

#### Control asynchronous execution

Never wait with a fixed timeout and assume the Workflow finished. Wait for a status,
step result, output, or final application state with a bounded polling helper. Use
the current Workflow test controls to bypass sleeps and retry delays, provide external
events, or force documented step failures. Fake timers do not model the Workflow
engine.

#### Test retries without erasing the behavior

Workflow steps can run again. Cover the costly failure mode: a retry must not create
duplicate charges, notifications, records, or messages. Disable the backoff rather
than the retry, make the relevant dependency fail at the intended point, and assert
the final durable state.

Mocking a step result is useful when testing only downstream orchestration, but it
skips the step implementation. Keep a separate test for the real operation and its
owned side effects. Do not mock every step and call the result a Workflow integration
test.

Follow the current disposal and cleanup requirements for Workflow test controls.
Leaked introspection state can invalidate isolation and make later tests observe an
already-running or completed instance.

## Mock HTTP with MSW

Use MSW when a network boundary must be replaced.

For server tests, its primary job is intercepting outbound calls to third-party
vendors such as payment, messaging, maps, OAuth, or email APIs. Use `setupServer` and
fail on unhandled outbound requests. This prevents tests from silently reaching the
internet or an unexpected vendor endpoint.

Model the part of the vendor contract the application owns:

- validate important request fields rather than accepting every payload;
- return representative documented successes and failures;
- capture outbound requests when assertions need to inspect them;
- keep ordinary handlers as suite defaults and override them per test for errors;
- reset handlers and captured state after every test;
- close the server after the suite.

Do not test that the vendor correctly implements its own API. The fake exists to test
what the application sends and how it responds to the documented contract.

MSW is also the preferred boundary for UI integration tests. Mock the API request,
not the data hook, API client, query cache, or component. That keeps serialization,
request construction, loading states, cache behavior, and error handling connected.

Use `setupWorker` in Browser Mode so interception happens at the browser's network
layer. Make unexpected application API calls fail; explicitly bypass only requests
the harness does not own, such as test-runner assets. Keep the mock API state small,
typed where practical, and reset it for every render or test.

## Run UI tests in a browser

Configure Vitest Browser Mode with a headless Playwright browser. Chromium is the
default unless browser compatibility is the behavior under test.

The browser suite should load production styles and mount realistic application
composition: router, query client, localization, theme, and other providers used by
the feature. Build one reusable render harness that accepts initial route and API
state. Prefer memory history over mocking navigation.

A real browser is valuable because layout, visibility, pointer events, responsive
CSS, image loading, WebSockets, and browser APIs remain real. Avoid recreating these
with a growing collection of jsdom polyfills.

Keep the browser deterministic:

- pin a useful default viewport and set another viewport only in tests that need it;
- load the same CSS the application loads;
- disable animation timing when it makes queries race, without removing layout CSS;
- set test environment variables explicitly so a developer's `.env` cannot select a
  different branch or load a real third-party script;
- disable retries in per-test query clients unless retry behavior is under test.

## Coverage

Coverage is a diagnostic map, not a target. Generate text, HTML, and LCOV reports,
but do not add blanket percentage thresholds or a CI gate by default. Look for
important use cases and surprising blind spots instead of chasing 100%.

Choose the provider that matches the runtime:

- Browser Mode normally uses V8 coverage exposed by Chromium;
- transformed or sandboxed runtimes may require Istanbul;
- Babel-native environments such as Expo commonly use Babel coverage.

Pin Vitest adapters and coverage packages to a version compatible with the installed
Vitest version. Include never-imported application files when the provider supports
it. Exclude generated code, vendored components, migrations, declaration files, and
test infrastructure when they would make the report less meaningful.

Do not let parallel shards overwrite one shared coverage directory. Run coverage
without sharding or configure collection and merging explicitly.

## Suggested structure

Adapt names to the project rather than forcing this exact tree:

```text
docker-compose.test.yml
vitest.config.ts
test/
  global-setup.ts       # database lifecycle, migrations, baseline state
  setup.ts              # MSW and per-test cleanup
  helpers/              # stable domain/API setup helpers
  msw/
    handlers.ts
    server.ts
src/test/
  render-app.tsx        # real providers, route, and per-test API state
  msw.ts                # browser worker and API handlers
```

Expose predictable scripts:

```text
test             run the suite once
test:watch       run the appropriate local watch mode
test:coverage    run once and write a browsable report
typecheck        validate the test and production code together
```

In a monorepo, provide these per application and one root command that runs every
suite. Keep each application's runner, environment, database lifecycle, and coverage
provider explicit; sharing conventions does not require pretending every runtime is
the same.

## Setup order

When bootstrapping a project, work in this order:

1. Identify the user-visible flows and the real system boundaries they cross.
2. Automate the disposable database lifecycle and prove a test can use it.
3. Add a server integration harness through the production entry point.
4. Add strict MSW handlers for third-party HTTP.
5. Add Browser Mode, production providers, and a reusable UI render harness.
6. Add MSW-backed UI integration tests around real user flows.
7. Add small unit tests for complex pure logic.
8. Add coverage reporting after the suite is trustworthy.
9. Add a small number of end-to-end tests only for critical flows that the two
   integration layers cannot cover together.

The setup is complete when a fresh checkout can run one command and receive a
deterministic result without manually preparing data, starting services, editing
environment files, or accessing the public internet.
