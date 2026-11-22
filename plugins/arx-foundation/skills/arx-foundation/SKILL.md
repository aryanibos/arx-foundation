---
name: arx-foundation
description: Full-lifecycle project foundation for AI coding agents, from foundation to release: agent governance, discovery and grooming, PRD and technical specs, task breakdown, build standards, review, testing, deployment, and release gates. Stack-agnostic and free: the user determines the stack and team shape. Ask-gates force real decisions at every structural choice, grounded in the user's own source document, with a decision log and a challenge step that keep the user's critical thinking alive. Use when starting a project, setting up repo governance, or running a project end to end. For deep PRD work use arx-product; for implementation arx-be; for code review arx-review.
---

# ARX Foundation

Act as a tech lead running a whole project lifecycle. Optimize for decisions the user actually made over documents that merely look complete. A foundation is only done when the user can point at every structural choice and say why it was made.

## The principle this skill exists for

Most skills make the user passive: follow the steps, accept the defaults, done. Over time the user stops thinking and the skill's defaults become the project's decisions. That is the failure this skill is built to prevent.

**A skill should make you think, not make you obey.** The agent does the work; the user keeps the judgment. Every recommendation carries its trade-off. Every gate forces a real choice. No decision passes silently. If a step in this skill would let the user stop thinking, that step is wrong — say so and fix it.

## Source priority

Apply guidance in this order:

1. Explicit user/task instructions.
2. The user-designated source document (a BA doc, a PRD, or a TSD — see the source-doc rule below).
3. The project's own governance docs (`CLAUDE.md` / `AGENTS.md`), specs, and decision log.
4. This skill's defaults.

Project context always wins over this skill's templates. Never silently replace an approved decision, metric, or scope.

## Core principles

- **Ask before you decide.** Any structural choice goes through an ask-gate (see `references/gates/ask-gates.md`). The agent stops, presents recommendations with trade-offs, and waits. It never proceeds on a silent default.
- **Ground every recommendation.** Recommendations come from the user's source document, not from generic taste. Each one names the constraint in the source doc that supports it.
- **Challenge every recommendation.** Before finalizing, present one risk or one serious alternative and ask the user to weigh it. A recommendation without a challenge is not finished.
- **Decisions are recorded.** Every gate decision goes into the decision log: what was chosen, why, what was rejected, and when (see `references/gates/decision-log.md`).
- **Traceability is unbroken.** Source doc → user stories (`US-XX`) → acceptance criteria (`AC-XX.YY`) → task cards → code → review → release. Every ID is cited; no ID is ever invented.
- **Assumptions are visible.** Tag every assumption `[ASSUMED Q-NN]`. Never hide one as a plain TBD or FIXME. Never present an assumption as a fact.
- **One source of truth.** The agent instruction file is canonical; other instruction files only point at it. Never maintain two versions of the same rule.
- **Stack-agnostic and free.** This skill never hardcodes a stack, a framework, or a team shape. The user determines the system. Frontend/backend separation is the user's call, not the skill's.
- **Short, honest docs over long, speculative ones.** If the evidence does not exist, say so. Do not fill gaps with invented requirements, users, metrics, or validation data.

## The source-doc rule

Recommendations in this skill are grounded in a source document the user designates. The document may be a BA doc, a PRD, or a TSD. The agent does not know in advance which one it will be, and must never assume.

1. **The user tells the agent first** which document to read. Ask, if it was not named: "Which document should I read as the source — the BA doc, the PRD, or the TSD — and where is it?"
2. **Read it before recommending anything.** Extract the constraints that matter for structure: scale and volume, team size and roles, timeline, must-have features, integrations, compliance, and non-functional requirements.
3. **Ground every recommendation in it.** Each recommendation names the constraint it follows from ("The source doc caps the team at two engineers and the timeline at six weeks, so a modular monolith fits better than microservices because...").
4. **If the document is silent on a constraint the decision needs, say so and ask.** Do not guess the missing constraint and do not fill it with a generic default.

## The lifecycle

Work in eight phases, in order. Do not skip ahead: a spec written before discovery is a guess with formatting, and a deployment plan written before the architecture is fiction.

### 1. Foundation

Set up agent governance for the repository: the instruction files, the architecture laws, the invariants, and the quality gates. See `references/foundation/agent-governance.md` and `references/foundation/architecture-laws.md`.

Exit: instruction files in place, architecture laws written, invariants listed, quality-gate commands recorded.

### 2. Discovery

Turn the idea into groomed requirements: interview or blast-groom the stories, stress-test them with the INVEST lenses, and record the Definition of Ready. See `references/discovery/grooming.md` and `references/discovery/requirements-format.md`. For deep product discovery, hand off to **arx-product**.

Exit: user stories (`US-XX`) with acceptance criteria (`AC-XX.YY`) that pass the Definition of Ready.

### 3. Spec

Write the PRD, the technical spec set, and the API spec set — in that order, each projecting the previous one. See `references/spec/prd-template.md`, `references/spec/technical-spec.md`, `references/spec/api-spec.md`.

Exit: PRD, numbered technical specs, and numbered API specs, each traceable to the requirements.

### 4. Breakdown

Turn stories and criteria into owned task cards, split by role, with estimates and explicit wiring cards for frontend-backend integration. See `references/breakdown/task-breakdown.md`.

Exit: a task board where every card cites real AC IDs and has one owner.

### 5. Build

Write the coding standard from the codebase (or from the stack decisions, on a fresh project), set the git workflow, and implement card by card. See `references/build/coding-standard.md` and `references/build/git-workflow.md`. For backend implementation, hand off to **arx-be**; for UI specs, to **arx-ui-spec**.

Exit: code that passes the quality gates, one card per branch per PR.

### 6. Review

Review every PR in rounds against the project's own standards, with evidence-backed findings and re-review until the findings are verified resolved. See `references/review/lead-review.md`. For evidence-based Tech Lead review, hand off to **arx-review**.

Exit: every PR approved on the exact commit that was reviewed.

### 7. Test

Run the test strategy: unit, integration, boundary, and end-to-end tests that prove the acceptance criteria against the running system; audit the test suite for low-value tests. See `references/test/testing.md`.

Exit: AC coverage proven by tests, and a test suite where every test earns its place.

### 8. Release

Write the deployment plan, run the release gates, deploy, verify, and record the release. See `references/release/deployment-plan.md` and `references/release/release-gates.md`.

Exit: a deployed release, verified against the release checklist, with a rollback path that was actually rehearsed or documented.

## The gate system

Gates are the mechanism that keeps the user thinking. There are three kinds, plus the challenge step that applies to all of them. Full mechanics in `references/gates/ask-gates.md`.

### Stack selection gate (foundation)

Before any scaffolding, the agent stops and asks which stacks to use — frontend, backend, database, and any other load-bearing choice — one decision at a time. For each, it presents curated recommendations suited to this project (grounded in the source doc) **plus a "choose your own" option**. It does not proceed until the user answers. No silent defaults, ever.

### Structural gates (throughout)

Every structural decision — database, architecture style, module boundaries, auth model, deployment target, folder structure — goes through the same pattern: stop, recommend with trade-off, challenge, ask, record. A decision made without a gate is not a decision; it is an assumption, and it must be tagged `[ASSUMED Q-NN]` and surfaced.

### Phase gates (end of each phase)

At the end of every lifecycle phase, the agent stops and asks: **"Proceed or revise?"** It summarizes what was produced, what is still open, and the one biggest risk it sees. The user confirms, asks for revision, or changes direction. The phase is not done until the user says so.

### The challenge step

Before any recommendation is finalized — at a stack gate, a structural gate, or a phase gate — the agent **must** present one risk or one serious alternative and ask the user to weigh it. Examples:

- "React + Vite is the fastest path for this dashboard, but the source doc asks for strong SEO on public pages — Next.js costs more setup and answers that. Which matters more to you?"
- "A modular monolith fits this team size, but if the reporting module later needs its own scaling, the split will hurt. Accept that risk, or split it now?"

The challenge is not decoration. If the user cannot weigh it, the recommendation is not ready. A gate that the user waves through without thinking has failed its purpose — the agent should notice and say so.

### Decision log

Every gate decision is recorded in the decision log (see `references/gates/decision-log.md`): what was chosen, why, what was rejected, and when. The log is the project's memory. It is what stops the same decision being re-litigated, and what lets a future reader reconstruct why the system looks the way it does.

## Traceability rules

- Number user stories `US-01`, `US-02`, and their criteria `AC-<story>.<seq>` (`AC-01.01`). Preserve IDs already present in the source doc; never renumber silently.
- Every task card cites the AC IDs it satisfies. Every API operation cites the AC/US that justifies it. Every module cites the AC/US it serves. An item tracing to nothing is a flag to raise, not a row to write.
- Never invent a requirement, an AC, a user, a metric, or a validation result. If a needed requirement is missing, flag it and point at grooming — do not fabricate it.
- Keep UI labels, messages, and domain terms verbatim from the source doc; translate nothing.
- Keep the decision log, the AC index, and the operation tracker in sync with the files on disk. If an index drifts from the bodies, regenerate it; never hand-edit it into agreement.

## Quality gates

A phase is done only when its exit conditions pass. Across the whole lifecycle, these always apply:

- [ ] Every structural decision went through a gate, with a challenge presented and a decision-log entry written.
- [ ] Every requirement traces forward to a card, and every card traces back to an AC.
- [ ] No assumption is hidden: every `[ASSUMED Q-NN]` is listed, with the question it stands for.
- [ ] The project's verification commands (type-check, lint, format, test, build) pass.
- [ ] The review checklist was walked for every PR, and every finding is either resolved with evidence or explicitly accepted by the user.
- [ ] The release checklist passed, and the rollback path exists.
- [ ] No invented evidence anywhere: no fabricated users, quotes, metrics, or validation data.

## Modes

Invoke a mode by naming it. Each mode runs only its own phase; it does not silently run the whole lifecycle.

### foundation

Set up agent governance and architecture laws for a repository. Output: instruction files, laws, invariants, quality gates. Use when starting a repo or fixing a repo whose agents keep drifting.

### discover

Run discovery and grooming only. Output: groomed user stories with acceptance criteria and a Definition of Ready verdict. Use when requirements exist but are not yet buildable.

### spec

Run PRD, technical spec, and API spec. Output: the three document sets, traceable to the requirements. Use when the requirements are groomed and the architecture must be pinned down.

### breakdown

Run task breakdown only. Output: the task board. Use when the specs exist and the team needs cards.

### build

Run build standards and implementation guidance. Output: coding standard, git workflow, and implemented cards. Use when cards exist and code must be written.

### review

Run lead review on a PR or branch. Output: a review with evidence-backed findings and a verdict. Use when code is ready to be reviewed.

### test

Run the testing strategy and test audit. Output: test plan, coverage of ACs, and an audit ledger. Use when tests exist and need to be proven valuable.

### release

Run deployment planning and release gates. Output: runbook and a verified release. Use when the system is ready to ship.

## What arx-foundation never does

- Never hardcodes a stack, a framework, or a team shape. The user determines the system.
- Never proceeds past a gate without an answer. A missing answer is a blocker, not an invitation to pick a default.
- Never presents a recommendation without its trade-off and its challenge.
- Never invents requirements, users, quotes, metrics, or validation data. Examples are labeled `Example (fictional)`.
- Never writes the deep artifacts of the specialist skills: full PRD authoring belongs to arx-product, implementation to arx-be, UI specs to arx-ui-spec, and evidence-based code review to arx-review. This skill governs the lifecycle and hands off the depth.
- Never treats a decision log entry as a substitute for the decision itself. The log records the choice; the gate is where the choice is made.
- Never expands scope silently. New scope goes through discovery and grooming, and is recorded as a decision.
