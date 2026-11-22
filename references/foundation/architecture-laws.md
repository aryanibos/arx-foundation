# Architecture Laws

How to write enforceable architecture laws for a project — the invariants that keep every agent and every engineer inside the same shape.

## What an architecture law is

A law is a rule about structure that is stated once, in the canonical instruction file, and enforced by review. It is not a suggestion and not a style preference. If a change breaks a law, the change is wrong, not the law — unless the law itself is revised through a decision-log entry.

## The laws every project needs

Adapt the list to the stack; the categories are the same everywhere.

### Boundaries and modules

- Name the allowed modules, exactly. Nothing else may become a module. State what is *not* a module (a sub-entity owned by an existing module) so nobody invents one.
- Layered architecture: thin handlers/routes → services (business rules) → repositories (data access). Handlers never query data directly. Frontend code never calls the network directly; all calls go through one typed client.
- A module is **deep** when a lot of behaviour sits behind a small interface — and "interface" means everything a caller must know (invariants, error modes, ordering, config), not just the signatures. Push back on shallow modules whose interface is nearly as wide as their implementation.
- Apply the **deletion test** to every proposed boundary: imagine deleting the module. If the complexity just moves to its callers, the boundary earns its keep. If complexity vanishes, it was a pass-through and the boundary is wrong.

### Ports and adapters

- Where the answer to "who owns this dependency" is a remote service or a third party, define a **port** at the seam and inject the transport as an **adapter**, so the logic stays in one deep module testable behind an in-memory adapter.
- Only introduce a port where two adapters are actually justified (production plus test). A single-adapter seam is just indirection.
- Time, randomness, and external I/O go behind ports so tests can inject them. Time-dependent tests never call the wall clock directly.

### State machines

- Every entity whose status changes under business rules gets a state machine: every state, every legal transition with its actor and guard, the terminal states, and the error for any other move.
- All transitions go through one function. No caller changes status by writing a field directly.

### Shared constants and contracts

- Business constants (limits, durations, fees, thresholds) live in one shared module and are imported everywhere. They are never environment variables and never duplicated as literals.
- Contracts shared between frontend and backend (schemas, enums, error codes) live in one shared package. Duplicating a literal on the other side is a bug, not a shortcut.

### Security

- No secrets in the repository or in logs. Ever.
- Operational endpoints (reset, job triggers) are **absent** in production, not merely access-controlled: the route does not exist there.
- Tokens are hashed at rest, single-use where they grant an action, and never logged.
- Personal data never enters logs or dashboards. Access to user data is limited to what the acceptance criteria require.

## Writing laws well

- One law, one sentence, one rationale. "Handlers contain no business logic, because business rules must be testable without HTTP."
- Give each law an ID when the project is large enough to cite them in review (e.g. `SEC-01`). The review checklist cites the ID; the reviewer never re-argues the law.
- Laws are decided at the foundation phase gate and recorded in the decision log. Changing a law later is a decision, not an edit: it goes through a gate with a challenge.
- If a law cannot be enforced by a tool, enforce it by review — and say so. A law nobody checks is a wish.
