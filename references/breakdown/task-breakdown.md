# Task Breakdown

How to turn user stories and acceptance criteria into a sprint-by-sprint, role-assigned task board.

## Gather the source of truth first

Read the business docs first: user stories, acceptance criteria, and the sprint breakdown. Pull sprint goals, US, and AC IDs from there. Also skim the technical specs and API specs to cite authoritative sections per card. If those paths do not exist, ask for the source rather than inventing stories. Sprint numbers and goals follow the sprint breakdown; do not invent your own.

## Establish the team (grill)

Grill the user on team size and roles (e.g. 1 backend, 2 frontend, 1 tech lead) unless they already stated it or a team document records an existing composition. Map each role to a placeholder (`BE1`, `FE1`, `TL`). The team shape drives owner assignment and load-balancing, so resolve it before any card is written.

This skill never mandates a team shape. Frontend/backend separation, a full-stack role, or a solo developer are all the user's call — ask, do not assume.

## Break each sprint into cards

For every sprint goal, split the work by role. One card = one shippable, independently-verifiable slice owned by one person. Each card has:

- **Card ID** (`<BE|FE|TL|DB>-S<sprint>-<NN>`)
- **Title** (one action-oriented line)
- **Task description**
- **AC IDs** it satisfies (real IDs from the business docs — never invented)
- **Owner** (one owner per card)
- **Est** (developer-days, sized so a card fits comfortably within a sprint; split anything that does not)
- **Docs** (the authoritative spec sections it implements)

Sequence cards so contracts and shells exist before the work that depends on them. Balance the load so no one is idle. Respect an existing scaffold/work model if the repo documents one; do not re-plan it unless asked.

## Make wiring an explicit card

The recurring failure this exists to fix: a feature's backend card and frontend card both exist, but the **integration** — pointing the real frontend at the real backend, replacing the mock/scaffold, handling the live error envelope, verifying the AC end-to-end against the running server — belongs to no card and no one owns it. It falls through the gap and the AC is never actually proven.

For every feature where backend and frontend are separate cards, decide whether the seam needs its own wiring card. Create one when integration is non-trivial: contract mismatch risk, real error/empty/loading states to map, auth/permission interplay, or an end-to-end AC that only passes once both sides are live.

Ownership rule: **assign the wiring card to whoever owns the last task that unblocks it.** Walk the dependency chain; the wiring card depends on both sides, so its owner is the owner of whichever side finishes last. State the unblocking card in the task description.

A wiring card is real work: a Card ID, "wiring" in its title, an Est, the AC IDs it actually proves end-to-end, and a Docs reference. Prefer to attach an end-to-end check that proves the AC against the running dev server as the card's exit criterion — a realistic scenario of medium or high complexity, with a failure or permission path, not only the simplest success case.

## Write the board

Write to `docs/task-breakdown/` (create or edit in place): `_index.md`, `team-and-process.md` (with the Definition of Done), and one `sprint-N.md` per sprint. If the folder exists, read it first and edit in place, preserving sprints or cards you were not asked to change. After writing, report which cards were added or changed.

Rules:

- Tech lead judgment, grounded in the docs; never invent AC or stories. If an AC is missing for work you think is needed, flag it (point at grooming) rather than fabricating one.
- One owner per card. Estimates in developer-days.
- Deployment, release, and ops procedures are not cards here; they belong in the deployment plan.
