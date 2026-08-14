# Testing setup

Optional blueprint for adding tests to a new or untested project. Testing principles
remain in `AGENTS.md`.

## Defaults

- Prefer Vitest.
- Prefer integration tests through public boundaries.
- Unit-test complex pure logic.
- Reserve E2E tests for critical flows integration tests cannot cover.
- Use a runtime-native runner when Vitest reduces fidelity.
- Make one test command own setup, execution, and teardown.

## Setup

### 1. Database

Run the production database engine in a test-only Docker Compose service with:

- test-only credentials and database name;
- an overridable port and health check;
- ephemeral storage;
- concurrency-safe project or container names;
- a guard against non-test connection strings.

The test command must start the database, wait for health, run real migrations, reset
state, run tests, and tear down after success or failure. The application and
migrations must use the same database.

Use transactions when the tested code shares them. Otherwise create unique state and
clean it through factories, public APIs, or domain helpers. Tests must not depend on
order or residue.

### 2. Server integration

Call the production HTTP, Worker `fetch`, RPC, or equivalent entry point. Keep
routing, validation, authentication, persistence, and serialization in the test. Use
the real database; do not mock the router, ORM, or service graph.

### 3. HTTP boundaries

Use MSW for replaced HTTP boundaries, mainly third-party vendors.

#### Server

- Use `setupServer`.
- Fail on unhandled requests.
- Validate important outbound fields.
- Model documented success and failure responses.
- Reset handlers and captured state after each test.

#### Browser

- Use `setupWorker`.
- Mock the HTTP API, not hooks, clients, caches, or components.
- Fail on unexpected application requests.
- Reset API state after each test.

### 4. Browser integration

Use Vitest Browser Mode with Playwright and Chromium. Load production styles and real
providers such as routing, data fetching, localization, and theme. Provide one render
helper that accepts route and API state.

Use Testing Library and `user-event`. Query by role, label, and visible state. Set
viewport and environment variables explicitly. Disable animations or retries only
when they are not under test.

### 5. Coverage

Generate coverage reports without blanket thresholds or a CI gate. Use the provider
supported by the runtime, include never-imported application files when possible,
and exclude generated code, migrations, declarations, vendors, and test
infrastructure. Prevent parallel shards from overwriting one report directory.

## Cloudflare

### Workers

Do not copy a harness or configuration from another project. Follow the current:

- [Workers testing overview](https://developers.cloudflare.com/workers/testing/)
- [Vitest recipes](https://developers.cloudflare.com/workers/testing/vitest-integration/recipes/)
- [Test APIs](https://developers.cloudflare.com/workers/testing/vitest-integration/test-apis/)
- [Known issues](https://developers.cloudflare.com/workers/testing/vitest-integration/known-issues/)

Use Cloudflare's supported local runtime, real Wrangler configuration, local bindings,
and fake credentials. Block production and remote resources. Keep host orchestration
outside the Worker runtime. Use MSW for outbound third-party HTTP, not platform
bindings.

### Workflows

Starting a Workflow confirms acceptance, not completion.

Cover:

1. Domain operations directly.
2. The request, scheduled, queue, or event boundary that starts the Workflow.
3. Important Workflow paths in the runtime, including output or durable side effects.

Use the documented Workflow introspection and control APIs instead of fixed sleeps.
Test retry idempotency for costly side effects and dispose of Workflow test state.
Do not add a synchronous production path for tests or mock every step in an
integration test.

Follow the current:

- [Workflow test APIs](https://developers.cloudflare.com/workers/testing/vitest-integration/test-apis/#workflows)
- [Workflow recipes](https://developers.cloudflare.com/workers/testing/vitest-integration/recipes/)
- [Rules of Workflows](https://developers.cloudflare.com/workflows/build/rules-of-workflows/)

## Project interface

### Scripts

```text
test
test:watch
test:coverage
typecheck
```

### Done

A fresh checkout runs deterministic tests with one command and no manual data setup,
service startup, environment edits, or public internet access.
