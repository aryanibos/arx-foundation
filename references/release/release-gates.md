# Release Gates

The checklist a release must pass before it ships, and the verification that proves it passed.

## The release checklist

A release is done only when every item passes. The list is confirmed with the user at the release phase gate; items that do not apply are marked `N/A` with a reason, never silently skipped.

- [ ] All cards in the release scope are done per the Definition of Done.
- [ ] Every AC in scope is proven by a test against the running system.
- [ ] The verification commands pass on the exact commit being released (type-check, lint, format, test, build).
- [ ] Every PR in the release was approved on the exact commit it contains; no approval carried forward across new commits.
- [ ] The decision log is up to date: every gate decision for this release is recorded.
- [ ] The operation status tracker (API specs) matches reality: every operation in the release is marked `OK` or explicitly deferred.
- [ ] The deployment plan covers this release: migrations (if any) run safely against the new image, the rollback path exists, and the rollback was rehearsed or is documented step by step.
- [ ] Secrets and configuration for the target environment are injected, not committed. No secret appears in the release artifact or its logs.
- [ ] The health endpoint reports healthy, including every critical dependency, after deployment.
- [ ] The release is versioned and tagged. The tag points at the exact commit that was verified — never at a branch tip that moved afterwards.
- [ ] Release notes exist: what changed, what was deferred, and the known issues with their workarounds.

## Versioning

Use semantic versioning: `MAJOR.MINOR.PATCH`.

- `MAJOR` for incompatible changes (removed or breaking API, data, or behaviour).
- `MINOR` for backwards-compatible additions.
- `PATCH` for backwards-compatible fixes.

A `0.x` version signals the API and behaviour are still stabilizing. The first stable release is `1.0.0`. Never re-tag a version: a bad release gets a new version, not a moved tag.

## Verification after deployment

Deploying is not verifying. After the release is live:

1. Run the health check and confirm every dependency reports healthy.
2. Run the smoke path: the one or two end-to-end flows that prove the release works for a real user.
3. Check the logs for errors in the first minutes after deploy.
4. Confirm the rollback path is still available (the previous image is retained and runnable).

If any step fails, roll back first and diagnose second. A broken release that stays live while the team investigates is a second incident, not a fix.

## Release record

Record every release in the decision log (see `../gates/decision-log.md`): version, tag, commit, date, scope (cards and AC IDs), and the verification result. The record is what lets a future reader answer "what shipped, when, and what did we know about it?"
