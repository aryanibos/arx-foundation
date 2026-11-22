# ARX Foundation

[![Version](https://img.shields.io/badge/version-1.0.0-111827)](https://github.com/aryanibos/arx-foundation/releases)
[![License: MIT](https://img.shields.io/badge/license-MIT-16a34a.svg)](LICENSE)
[![Codex Plugin](https://img.shields.io/badge/Codex-plugin-111827)](https://developers.openai.com/plugins/)
[![Agent Skill](https://img.shields.io/badge/Agent%20Skill-cross--agent-7c3aed)](https://skills.sh/)

**ARX Foundation** is a cross-agent project-foundation skill for AI coding agents. It takes a project from foundation to release: agent governance, discovery and grooming, PRD and technical specs, task breakdown, build, review, testing, deployment, and release — with the user making every structural decision along the way.

The skill is named **`arx-foundation`**.

> A skill should make you think, not make you obey. This skill asks before it decides, shows the trade-off before it recommends, and never lets a default pass silently.

## Install

### Cross-agent install

Install from GitHub with the Skills CLI:

```bash
npx skills add aryanibos/arx-foundation
```

The cross-agent entrypoint is:

```text
skills/arx-foundation/SKILL.md
```

This distribution is intended for compatible AI coding agents such as **Codex, Claude Code, Cursor, OpenCode**, and other clients that support Agent Skills.

### Codex plugin marketplace

Add this repository as a Codex plugin marketplace source:

```bash
codex plugin marketplace add aryanibos/arx-foundation
```

Then install **ARX Foundation** from that marketplace.

## What ARX Foundation does

`arx-foundation` behaves like a tech lead running a whole project lifecycle, not like a template generator. It is designed for:

- setting up agent governance for a repository (one source of truth, architecture laws, context recovery)
- discovery and grooming of requirements into buildable user stories
- PRD, technical, and API specifications
- task breakdown into owned, traceable cards
- build standards, git workflow, and code review
- testing strategy and test audit
- deployment planning and release gates

The skill is **stack-agnostic and free**: it never hardcodes a stack or a team shape. The user determines the system. At foundation, the agent stops and asks which stacks to use, presenting recommendations grounded in the user's own source document (a BA doc, a PRD, or a TSD) plus a "choose your own" option — and it does not proceed until the user answers.

## The lifecycle

```text
Foundation → Discovery → Spec → Breakdown → Build → Review → Test → Release
(governance,   (grooming,   (PRD, tech,   (cards,    (code,    (rounds,   (strategy,  (deploy,
 laws, gates)   US/AC)       API specs)    owners)     standards) evidence)  audit)     gates)
```

Every phase ends in a **phase gate**: the agent stops and asks "proceed or revise?" Every structural decision passes through an **ask-gate** with a recommendation, a challenge (one risk or alternative the user must weigh), and a record in the **decision log**. Nothing is decided silently.

## The differentiator

Most skills make the user passive: follow the steps, accept the defaults, done. `arx-foundation` is built to keep the user's critical thinking alive. The agent does the work; the user keeps the judgment. Every recommendation carries its trade-off, every gate forces a real choice, and every decision is recorded with its reason and its rejected alternatives. If a step would let the user stop thinking, the skill is wrong — fix the skill, not the user.

## Traceability

Requirements trace forward: source document → user stories (`US-XX`) → acceptance criteria (`AC-XX.YY`) → task cards → code → review → release. Every ID is cited; no ID is ever invented. Assumptions are tagged `[ASSUMED Q-NN]`, never hidden as plain TBD or FIXME.

## References

- `references/foundation/agent-governance.md` — one source of truth, agent instruction files, context recovery.
- `references/foundation/architecture-laws.md` — writing enforceable architecture laws, module boundaries, ports and adapters.
- `references/discovery/grooming.md` — INVEST lenses, interview vs blast mode, Definition of Ready.
- `references/discovery/requirements-format.md` — user stories, Gherkin acceptance criteria, IDs and traceability.
- `references/spec/prd-template.md` — compact PRD template.
- `references/spec/technical-spec.md` — numbered technical spec set, grill-first, module design.
- `references/spec/api-spec.md` — API contract format, operation surface, status tracker.
- `references/breakdown/task-breakdown.md` — cards, owners, estimates, wiring cards.
- `references/build/coding-standard.md` — rules inferred from the codebase, review checklist.
- `references/build/git-workflow.md` — one card = one branch = one PR, commit and PR conventions.
- `references/review/lead-review.md` — review rounds, severity, evidence, re-review.
- `references/test/testing.md` — test strategy, boundary tests, test audit ledger, E2E.
- `references/release/deployment-plan.md` — environments, infra grill, rollback, runbook.
- `references/release/release-gates.md` — release checklist, versioning, verification.
- `references/gates/ask-gates.md` — gate mechanics: stack selection, structural gates, challenge step, source-doc rule.
- `references/gates/decision-log.md` — decision log format.

## Relationship to the ARX family

`arx-foundation` governs the lifecycle. For deep work inside a phase, hand off to the specialist skills:

- **arx-product** for product discovery and full PRD authoring.
- **arx-ui-spec** for turning visual references into UI specifications.
- **arx-be** for backend implementation.
- **arx-review** for evidence-based Tech Lead code review.

## License

MIT — see [LICENSE](LICENSE).
