# Technical Spec

How to produce the technical specification set: the architectural keystone the rest of the pipeline depends on. It runs after the business docs exist and before the API specs, task breakdown, and deployment plan, all of which depend on the decisions made here.

## Output

A **numbered file set** under `docs/technical-specs/`, not one monolith (`_index.md` plus `NN-topic.md` files), so sections are linkable from task cards and reviews. The standard core, in order:

1. Overview
2. System architecture
3. Repository structure
4. Tech stack
5. Module definitions
6. Data model
7. Security
8. Non-functional requirements
9. Authentication and authorization
10. Integration points
11. Environment configuration
12. (Ad-hoc trailing specs — see below)

Number sections within each file (`## 6.1`) so they are citable as stable anchors. Keep `_index.md` and the file numbering consistent; if you add or reorder files, update the index.

## Phase 1: Grill (stack and tooling first)

Read the source of truth before asking: the business docs, plus any existing instruction files, README, or partial specs. The business docs define *what* is built; this document defines *how*. Then interview the user one question at a time, recommending an answer for each and explaining the trade-off, until every architectural branch is resolved.

Discipline for the grill:

- **One decision per turn.** Do not paste a questionnaire at the user. Ask the live question, give your recommended answer and the one reason it wins over the runner-up, and wait.
- **Walk the branch the answer opens.** Each answer changes what to ask next. Follow the branch the user's choice creates before starting a new one.
- **Confirm before moving on.** Restate the decision in one line and get explicit agreement before leaving it — especially for the hard-to-reverse ones (architecture style, module boundaries, primary datastore).
- **Stack and tooling is mandatory and comes first.** It cascades into every other section: runtime and language, backend framework and API style, frontend framework and rendering model, datastores (primary DB, cache, queue, object storage), migration and seed mechanism (one stable migrate command, one seed command), repository shape (monorepo vs polyrepo), tooling (package manager, type-check, lint, test, CI, e2e), infra and integrations.
- **Architecture style and module boundaries are the hardest to reverse: grill them hardest.** Do not accept the first style the user names: lay out the realistic options against *this* project's NFRs and team size, give a recommendation with the trade-off, and get explicit buy-in. Then derive module boundaries from the bounded areas in the business docs one at a time.
- **Design each module to be deep** (see `../foundation/architecture-laws.md`): a lot of behaviour behind a small interface. Apply the deletion test to every boundary. Classify cross-module dependencies and define ports only where two adapters are actually justified.
- Surface contradictions against the business docs as you go, and flag decisions that are hard to reverse.

For the one or two riskiest module interfaces, offer to **design it twice**: draft several different interfaces, compare by depth and seam placement, and let the user pick before the shape is locked into the spec.

## Phase 2: Write the set

- **Module definitions:** one per bounded area; responsibility, public surface, and the AC/US it serves.
- **Data model:** every entity, field, type, nullability, default, key, relationship; the ERD; common columns; a state machine for each entity whose status changes (every state, every legal transition with actor and guard, terminal states, the error for any other move).
- **Security and auth:** authn mechanism, authz model (roles/RBAC), session/token strategy, secrets, and each trust boundary with its threats and controls.
- **NFRs:** volume, throughput, latency, availability targets, scaling assumptions — each with a source.
- **Integration points:** each external system, its contract, failure mode, and fallback.
- **Environment configuration:** the full env-var set per environment.
- **Operational endpoints:** every project gets two by default — a health-monitor endpoint reporting liveness and per-dependency status, and a reset-to-known-state endpoint that is **mounted only in non-production environments and absent in production**.
- **Ad-hoc trailing specs:** one numbered doc per cross-cutting concern needing focused attention (a search strategy, a tricky state machine, a performance-sensitive path). Propose each candidate to the user and confirm before adding it. Each ad-hoc doc opens with "cite this, don't repeat it" and holds a decision matrix, the exact contract, performance targets, constraints, non-goals, cross-references, and open follow-ups.

## Writing rules

- Trace every module and data-model decision back to the AC/US it serves. Where the business docs are silent, raise it during the grill rather than guessing.
- Preserve domain terms and non-English UI labels verbatim; mirror the glossary.
- If a spec set already exists, read it and update affected files in place rather than clobbering; report what changed.
- On a running project (adopt mode), describe the system as built: read the code, config, schema, and deploy files, and record every gap or inconsistency as a Finding in the section it belongs to.
