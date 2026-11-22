# Agent Governance

How to set up the instruction files that govern AI agents in a repository, so agents stay inside the project's rules instead of inventing their own.

## The single-source-of-truth pattern

One file is canonical. Every other instruction file only points at it.

- **`CLAUDE.md`** (or the equivalent for the agent family in use) is the canonical file. It holds the project overview, the architecture laws, the team and work model, the skill map, the testing and verification commands, and the git workflow. It is the only file that defines rules.
- **`AGENTS.md`** is the coordination file. It says `Read @CLAUDE.md`, then adds only what CLAUDE.md does not: scope in one paragraph, roles, and invariants for multi-agent coordination. It defines no product, stack, or workflow of its own.
- **`GEMINI.md`** (or the equivalent for another agent family) is the adapter file. It says the agent follows the same rules as CLAUDE.md, then restates the must-follow rules in compact form plus a "do not do" list. It defines nothing new.

If a rule appears in two files, it will drift. A rule lives in exactly one place; other files cite it.

## What the canonical file must contain

- **A critical banner at the top.** "These rules apply to every task. If context is compacted, re-read this file before continuing. Do not mark work done before the relevant verification passes." Agents lose context; the banner tells them how to re-enter.
- **Project overview.** What the system is, one paragraph, plus the stack table (runtime, language, frontend, backend, database, validation, lint, test).
- **Source of truth.** Where the current scope lives (the requirements document, the decision log), and what "done" means for scope. State explicitly what a status marker means (e.g. `TODO` in a tracker means "not implemented", not "not agreed").
- **Team and work model.** Roles, what each role owns, and the rules of the work model: one card = one branch = one PR; who reviews whose work; sprint cadence.
- **Skill map.** Which skill to use for which task, and which skill *not* to use for it. A table, not prose. The map is read before code is changed.
- **Architecture laws.** The enforceable invariants (see `architecture-laws.md`).
- **Testing and verification.** The exact commands (type-check, test, lint, build) and the rules that tests must follow (e.g. boundary values, no direct wall-clock calls in time-dependent tests).
- **Git workflow.** Branch pattern, commit format, PR template requirements, forbidden commands (e.g. no `git add -A`, no force push).
- **Context recovery checklist.** A checkbox list an agent walks after its context is compacted: project identity, foundation status, current baseline, figure/design verification status, security rules, writing style, and which skill to use for what. This is what makes a long project survivable across sessions.

## Invariants

Invariants are the rules that must never be violated, written as a short numbered list in the coordination file. Good invariants:

- Do not invent requirement IDs; sync only IDs and text present in the source.
- One card = one branch = one PR, with the branch pattern named.
- Copy (UI text, messages) follows the acceptance criteria verbatim.
- Operational endpoints are absent in production, not merely access-controlled.
- Claims of evidence (a graph, a test result, a measurement) only when the tool actually returned it. Never fabricate proof.

## Rules for writing governance

- Write rules as imperative sentences a reviewer can cite verbatim: "Handlers contain no business logic." Not paragraphs.
- Name the document that owns each rule. If the coding standard owns commit format, the governance file cites it; it does not restate it.
- Keep the canonical file under ~300 lines. When it grows past that, the detail belongs in the `docs/` sets it points to, not in the file itself.
- Review the governance files at every phase gate. A rule the team stopped following is worse than no rule: either enforce it or change it, and record the change in the decision log.
