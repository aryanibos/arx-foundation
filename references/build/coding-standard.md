# Coding Standard

How to author a project's coding standard and review checklist: one file per section, each rule citable verbatim by a reviewer.

## Output

Two document sets:

- `docs/coding-standard/`: the rules, each with a short rationale and a correct/incorrect example.
- `docs/code-review-checklist/`: a binary checklist a reviewer walks per PR, each item traceable to a rule.

## 1. Read the project first

The standards must describe **this** codebase, not a generic ideal. Before writing anything:

- **README** and `CONTRIBUTING.md`: declared conventions, build/run/test commands, project purpose.
- **Instruction files** (`CLAUDE.md` / `AGENTS.md`): often the real, enforced rules.
- **Manifests and tool config**: package manager files, linter/formatter/type-checker config. These are existing, machine-enforced standards: capture them, do not reinvent them.
- **The code itself.** Sample 15 to 30 representative source files across layers. Extract the real patterns: naming, file/folder layout, error handling, layering boundaries, test structure, import conventions, comment density, file-size norms. Note where the codebase is internally inconsistent; those are decisions the user must make.
- Any existing architecture docs, decision logs, or a prior standards file to extend rather than replace.
- The security section of the technical specs: its threat model and control IDs. Each control becomes one checklist item.

If the repo is empty or near-empty, switch to interview mode: ask the user for stack, architecture, and preferences, and produce a starter standard from their answers.

## 2. Decide the rules

For each area, state the rule the codebase already follows. Where the code is silent or inconsistent, mark it as an open decision and grill the user on it, one question at a time, recommending a default for each. Do not pad with rules irrelevant to the project.

- **Language and types:** version, type-safety rules (no `any`, null handling, casts), enums vs unions.
- **Naming:** files, folders, functions, types, constants, DB columns. Give the pattern and an example each.
- **Project structure and layering:** where code lives, allowed dependencies between layers, what must not appear where (e.g. no business logic in handlers, no direct DB access outside a repository layer).
- **File and function size:** limits and the split strategy when exceeded, only if the project cares.
- **Error handling:** domain errors vs raw throws, how errors surface to the boundary, logging.
- **Async / concurrency / transactions:** atomicity rules, race handling, where relevant.
- **Dependencies and tooling:** package manager, banned/preferred libraries, the exact verification commands (type-check, lint, format, test, build).
- **Tests:** framework, structure, what must be tested, isolation/mocking rules, coverage expectations.
- **Security and secrets:** config access, secret handling, input validation, authz enforcement.
- **Comments, commits, docs:** comment policy, commit format, API/schema docs.
- **Formatting:** defer to the formatter config; state the command, do not restate rules the formatter enforces.

Propose stack-agnostic baseline rules as defaults, adapted to this codebase, and let the user confirm or adjust each.

## 3. Write the standard

- Number the headings inside each file (`## 2. Naming`, `### 2.1 Files`) so the checklist and reviewers can link to stable anchors.
- Each rule: imperative statement + one-line rationale + a correct/incorrect code block in the project's language.
- Quote-able and verbatim-citable: short, declarative sentences, not prose. Prefer "Handlers contain no business logic." over a paragraph.
- `_index.md` holds the one-paragraph project and stack summary, the verification commands, and the table of contents. It holds no rules.
- Mark rules the linter/formatter/CI already enforces as **(enforced)** so reviewers do not waste time on them by hand.

## 4. Write the review checklist

One binary item per rule, grouped by the same sections, each item traceable to its rule and to a security control ID where one applies. A reviewer walks it item by item per PR; an item is either met or it is a finding. The checklist is what the review phase consumes — a wrong or unowned rule here propagates into every future review, so confirm every rule with the user before committing it.

## Maintenance

A standard nobody follows is worse than none. Revisit the standard at every phase gate: a rule the team stopped following is either enforced or changed, and the change is recorded in the decision log.
