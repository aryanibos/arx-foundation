# API Spec

How to produce the API specification set: the interface contract for the system. It runs after `technical-spec` (architecture, modules, and data model must already exist) and before task breakdown, setup, and review, all of which cite endpoint contracts.

## Output

A **numbered file set** under `docs/api-specs/`, not one monolith (`_index.md` plus `NN-topic.md` files), so a single resource or operation is linkable from task cards, stubs, and reviews.

1. **`01-conventions.md`**: the transport contract shared by every operation — base address, content type, success and error envelope, pagination, the full status/fault-code table, and a role reference table. Every resource file cites it rather than restating it.
2. **`02-authentication.md`**: the login / refresh / logout (or token-issue) operations, including the token/cookie strategy.
3. **`NN-<resource>.md`**: one file per module/bounded resource from the technical specs. Order resources by dependency (foundational/master data first).
4. **`NN-system.md`** (last numbered file): the operational endpoints — health monitor and, where mounted, reset-to-known-state — with the rule that reset is registered only in non-production environments.
5. **`_index.md`**: version/base-address header, an **Operation Status Tracker** grouped by resource (legend: `OK` implemented and tested, `WIP` in progress, `TODO` not started, `SCAFFOLD` stub returning a mock), a files table, and links back to the technical specs and business docs. The tracker is the at-a-glance build state: the setup phase marks stubbed operations `SCAFFOLD`, the PR that implements an operation sets its row to `OK`, and review checks that row.

## Derive, do not invent

The api-specs are a *projection* of the technical specs, not new invention:

- Protocol/style comes from the tech-stack doc. Do not re-litigate it.
- Each module becomes one resource file; its listed operations become the documented operations.
- Request/response shapes, enums, and valid transitions come straight from the data model. Do not invent fields the data model does not have.
- Auth and the role matrix become the per-operation **Access** line.
- Every operation cites the AC/US that justifies it. An operation tracing to no AC/US is a flag to raise, not a row to write.

Confirm the protocol and the planned file list with the user before writing. Surface contradictions against the technical specs as you go. Only grill where the technical specs are genuinely silent on a contract detail (pagination defaults, idempotency keys, envelope shape); recommend a default for each and confirm.

## Design the operation surface for depth

An API operation is an interface in the strict sense: the full contract a client must know (inputs, outputs, errors, ordering, idempotency).

- Prefer **few deep operations** that do a meaningful unit of work over many shallow CRUD passthroughs that push orchestration onto every client.
- Apply the **deletion test** to each proposed operation: if removing it just makes callers compose two others, it was a pass-through; if removing it forces every caller to reimplement a rule, it earns its place.
- When the surface has real alternatives (RPC-style verbs vs resource transitions, coarse vs granular operations), draft a couple of radically different surfaces, compare by depth and by how much orchestration each leaves to clients, and let the user pick before writing the file.

## Writing rules

- Cite, do not restate: cross-link the technical-spec sections for authoritative field rules, role matrices, and state machines instead of duplicating them.
- Preserve domain terms and non-English UI labels and messages verbatim. Keep error `code` in English and user-facing `message` in the UI language.
- Number sections within each file so operations are citable as stable anchors.
- If an api-specs set already exists, read it and update affected files in place rather than clobbering; report what changed. Then check that `_index.md` matches the files on disk.
- On a running project (adopt mode), document the live surface from the code. Mark each place where the code and the technical spec disagree as a finding in that operation's file, and list the findings in your report. Do not change the contract here.
