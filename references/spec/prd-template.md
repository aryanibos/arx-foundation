# PRD Template

A compact PRD template for a feature or product. Fill every section or mark it `UNKNOWN` / `N/A` with a reason. A PRD missing a required section is a draft, not a PRD. For deep product discovery and full PRD authoring, hand off to **arx-product**.

## 1. Metadata

| Field | Value |
|---|---|
| Title | |
| Owner | |
| Status | Draft / In review / Approved |
| Source document | (BA doc / PRD / TSD the requirements came from) |
| Date | |

## 2. Problem

Who has the problem, what happens today, what it costs them, and what changes if it is solved. One paragraph, with a named source for every claim.

## 3. Users

The personas/roles, each with a source. No persona without evidence.

## 4. Goals and non-goals

- **Goals:** what this must achieve, in measurable terms where possible.
- **Non-goals:** what this explicitly does not do. Anything not listed as in scope is out of scope.

## 5. Success metrics

One north-star metric, plus at most three supporting metrics. Every metric names its instrument (how it will be measured). A metric without an instrument is marked `TBD`.

## 6. Scope

What is in for this release, and what is deferred. Pin a Phase 1 boundary so the work does not sprawl.

## 7. Functional requirements

One requirement per row, each traced to a goal or a user need. A requirement without a parent is cut or parked in Open Questions.

| ID | Requirement | Parent goal / user need |
|---|---|---|
| REQ-01 | | |

## 8. User stories

Stories in the format of `../discovery/requirements-format.md`, each with acceptance criteria in Given/When/Then. Every story has at least one criterion; every criterion is verifiable without asking the author.

## 9. Handoff notes

- **To design / UI spec:** screens, user flows, states (empty / loading / error / success), visual references.
- **To backend:** data model, API surface, business rules, acceptance criteria, non-functional requirements.

## 10. Open questions

What is unknown, and the next step to resolve each item. Unknowns are listed, not hidden.

## 11. Risks

Each risk with its likelihood, impact, and mitigation or acceptance.

## 12. Decisions

The decision log for this PRD (see `../gates/decision-log.md`): what was decided, why, what was rejected, and when.

## Quality gates

A PRD is done only when every item passes:

- [ ] Problem names the user, the current pain, and the cost of doing nothing — with a source.
- [ ] Goals and non-goals are both written down.
- [ ] Success metrics are measurable, or marked `TBD` with the instrument named.
- [ ] Every requirement traces to a goal or a user need.
- [ ] Every user story has acceptance criteria in Given/When/Then.
- [ ] Every number, quote, or user claim has a named source.
- [ ] Open questions list what is unknown and the next step to resolve each.
- [ ] Handoff sections are filled, or explicitly marked `N/A` with a reason.
- [ ] No invented users, quotes, metrics, or validation data anywhere.
