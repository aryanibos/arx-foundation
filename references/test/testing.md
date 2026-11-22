# Testing

Test strategy for a project run under this skill: what to test, how to prove acceptance criteria, and how to keep the suite worth its maintenance cost.

## Strategy

Test in layers, each layer proving something the layer below cannot:

- **Unit tests** prove business rules and boundary values in isolation (e.g. a fee function at 0, 1, 4, 5, and 6 days late). Boundary values are mandatory: every rule with a threshold gets a test at each side of it.
- **Integration tests** prove modules against real collaborators (database, queue, storage) with the ports' real adapters — or in-memory adapters where the architecture defines them.
- **End-to-end tests** prove acceptance criteria against the running system: a realistic scenario of medium or high complexity for the feature, with a failure or permission path, not only the simplest success case. One E2E flow per feature is the minimum; the wiring card owns it.

## Rules

- Tests that depend on time must inject a clock; never call the wall clock directly.
- Tests that depend on randomness must inject a seed or a generator; never assert on random output.
- A test asserts behaviour, not implementation: it must survive a refactor that preserves behaviour.
- The verification commands (type-check, lint, format, test, build) run as one quality gate. A test suite that passes while the gate fails is not green.
- E2E tests run against a seeded known state (dev seed or QA seed), with expected numbers recorded in the test, so a failure points at the system, not at the data.

## Acceptance-criteria coverage

Every AC gets at least one test that proves it. The mapping is explicit: the test file or the card names the AC IDs it covers. An AC with no test is unproven — report it as such at the phase gate, do not call the feature done.

## Test audit

A test earns its place when it protects a behaviour, a credible regression, or a contract that matters on its own. Audit the suite regularly:

1. **Scope.** Agree the scope (one module, one folder, one pattern). Pin the base commit and run the scope's tests once on it. A test that fails on the base is a possible product bug: reproduce and report it; never delete it.
2. **Discover, read only.** For each test in scope, read the whole test, the production code it covers, its callers, and the tests next to it. Hunt for junk: tests that assert nothing, duplicate a stronger test, couple to implementation details, or exist only to keep a test-only production seam alive.
3. **Write the ledger.** One row per test in a decision file (`docs/decisions/<date>-test-audit-<scope>.md`):

| Mark | Meaning | The row must name |
|---|---|---|
| R | Retain | The regression it catches |
| F | Fix the assertion, keep the contract | What the assertion misses today |
| C | Consolidate into another test | The keeper test that absorbs it |
| D | Delete | The keeper test that still proves the contract, or why no contract exists |

Judge a test by its assertions, not by its name. Show the C and D rows to the user and get approval before any edit.
4. **Edit one batch.** One coherent batch (one production owner and its tests). In the same change, delete the test-only exports, flags, wrappers, and dead production code the batch unlocks. Do not keep aliases for them.

Optimize for confidence, not for deletion count. Static or slow is not a reason to delete.
