# Deployment Plan

How to produce the operational runbook: one file per section, by first grilling the user on infrastructure, environments, secrets, release flow, and rollback until every gap is resolved.

## Output

`docs/deployment-plan/` with an `_index.md` and one file per section: environments, infrastructure overview, initial deployment, release updates, rollback, database reset, and a database debugging cookbook. It is operational, not aspirational: every command must be copy-paste-runnable for this project, using its real service names, paths, image refs, and env vars.

## Phase 1: Grill

Interview the user relentlessly about how this system actually deploys, one question at a time, walking each branch until resolved. For every question, give a recommended answer and explain the trade-off, but do not move on until they confirm.

**Inspect before asking.** Read the repo first: compose files, Dockerfiles, CI workflows, deploy configs, proxy configs, `.env.example`, migration setup, README, any instruction files, and the security and environment sections of the technical specs. Every fact you can read, read; do not ask what the repo already answers.

Cover, at minimum (skip what is irrelevant to the stack, probe what is load-bearing):

- **Environments.** How many (dev / test / staging / prod), their URLs/hosts, who can deploy to each, and how they differ.
- **Infrastructure.** Hosting, what runs where, the topology (app, db, cache, proxy, object storage), and the network boundaries. **Pin down whether the app and the database live on the same host or separate hosts**: if separate, capture how the app reaches the DB and whether DB commands must run from the DB host.
- **Artifacts and registry.** How images/builds are produced (CI? manual?), where they are stored, tagging scheme, and how a host authenticates to pull.
- **Configuration and secrets.** The full env-var set, which are required vs optional, how secrets are generated and injected, and what must never be committed.
- **Initial deploy.** Prerequisites on a fresh host, the exact bring-up sequence, first-run migrations, seeding, and the admin/bootstrap credentials.
- **Reverse proxy and TLS.** Proxy choice, domains, certificate issuance/renewal, and security headers.
- **Release updates.** The update flow with and without schema changes, how migrations run against a new image safely, version pinning, and how a deploy is verified. **Establish one migrate command and one seed command** that resolve their own paths and connection string.
- **Rollback.** How to revert an image, what happens to the database on rollback (destructive migrations? down-migrations? manual?), and who to contact.
- **Operations.** DB access for debugging, backup/restore, log access, and health checks.
- **Database state reset.** How the database is wiped, re-migrated, and reseeded back to a known initial state, and which seed each environment uses. Establish which environments allow a reset (dev / test / staging / UAT only; never prod), who triggers it, and the exact order of operations across hosts. Capture what must happen to the app after a wipe (restart, drain connection pool, clear cache).

Surface contradictions as you find them. Note anything genuinely dangerous (no backups before a destructive migration, secrets in the repo, no rollback path) and make sure the plan addresses it.

On a running project (adopt mode), document how the deployment runs today. Record each gap as a finding in the runbook and in your report. Do not redesign the deployment here.

## Phase 2: Write the runbook

- Commands must reflect reality: real container/service names, real image refs, real env-var names. No placeholders where a real value is known.
- Every destructive command carries a one-line warning about what it deletes and how to back up first.
- **Never document migrations or seeding as a hardcoded path or inline raw SQL.** The migrate and seed steps must invoke the project's own runner, which resolves the migrations location relative to its own module and reads the connection string from the environment. State them as one stable command each.
- Annotate, number, and order steps so an operator can follow top-to-bottom on a fresh host.
- If the runbook exists, read and update it in place rather than clobbering, and report what changed.
