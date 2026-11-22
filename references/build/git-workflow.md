# Git Workflow

Branch, commit, and pull-request conventions for a project run under this skill.

## One card = one branch = one PR

- Branch pattern: `<type>/<CARD-ID>-<kebab-title>` (example: `feat/BE-S1-02-auth-login`, `fix/FE-S2-03-dashboard`).
- One card per branch. Never mix two cards in one branch, and never commit unrelated work into a card's branch.
- The branch is created from the card; the PR closes the card.

## Commit format

Imperative English, with traceability trailers:

```text
<type>(<scope>): <short summary>

Refs: <CARD-ID>
AC: <AC-XX.YY>
```

- `<type>`: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`.
- `Refs:` names the card. `AC:` names the acceptance criteria the commit satisfies. Both are mandatory on work commits.
- Assumptions use tags, never plain TBD/FIXME: `[ASSUMED Q-NN]` (or `[COPY-TBD Q-NN]` for copy). The tag names the open question; the decision log answers it.

Forbidden, always:

- `git add -A` / `git add .` — stage exactly what the commit contains.
- Force push to a shared branch.
- Committing secrets (`.env`, dumps, credentials, storage files) — check staging before every commit.

## Pull requests

- Fill the PR template completely, including the security checklist (one binary item per control from the technical specs).
- The PR description links the card and the AC IDs it satisfies.
- A PR is reviewed in rounds against the project's own standards (see `../review/lead-review.md`). The author fixes; the reviewer re-reviews; findings stay open until verified resolved in code, tests, or docs — never because a commit message says they were fixed.
- A PR owned by the tech lead is reviewed by another engineer on the team, in the card's area. Nobody approves their own work.
- Update the operation status tracker in the API specs (`TODO` → `OK`) only for operations the PR actually implemented and tested.

## Definition of Done (per card)

A card is done when all of these hold:

- [ ] The AC IDs on the card pass against the running system.
- [ ] The verification commands pass (type-check, lint, format, test, build).
- [ ] The review checklist was walked and every finding is resolved or explicitly accepted.
- [ ] The tracker rows the card owns are updated.
- [ ] No assumption was hidden: anything undecided is tagged `[ASSUMED Q-NN]` and listed.
