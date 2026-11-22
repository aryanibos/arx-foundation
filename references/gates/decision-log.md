# Decision Log

The format for recording decisions. The log is the project's memory: it stops the same decision being re-litigated, and it lets a future reader reconstruct why the system looks the way it does.

## Where it lives

`docs/decisions/<YYYY-MM-DD>-<kebab-topic>.md`, one file per decision (or per decision session, e.g. a grooming session or a gate session). The file sits with the other decision logs. A single running log file is acceptable on a very small project; prefer one file per decision once the project has more than a handful.

## Format

Each entry answers five questions. One line each is enough; a paragraph is the maximum.

```markdown
# Decision: <short title>
**Date:** <YYYY-MM-DD>
**Status:** Decided / Reversed (see <new entry>)
**Context:** What forced the decision, in one or two sentences.
**Decision:** What was chosen.
**Reason:** Why it wins over the alternatives.
**Rejected:** What was considered and rejected, with one line each for why.
**Consequences:** What this commits the project to (and what it rules out).
```

## Rules

- **Every gate decision gets an entry.** Stack choices, architecture style, module boundaries, auth model, deployment target, folder structure, scope cuts, and releases. If it was decided at a gate, it is in the log.
- **Record the rejection, not just the choice.** A log that only says what was chosen cannot stop the rejected option from coming back. The rejected alternatives with their reasons are the valuable part.
- **A decision can be reversed, never edited.** To change a decision, write a new entry that names the old one and says why the new information or constraint changes it. The old entry's status becomes `Reversed (see <new entry>)`. History stays readable.
- **Assumptions are not decisions.** An `[ASSUMED Q-NN]` tag is an open question with a placeholder answer. When the question is answered, the assumption becomes a decision-log entry — or the work built on it is revisited.
- **Keep it short.** A decision log that nobody reads is a diary. One line per field, plain language, no filler.

## Example (fictional)

```markdown
# Decision: Use a modular monolith, not microservices
**Date:** 2026-11-22
**Status:** Decided
**Context:** The source doc caps the team at three engineers and the first release at eight weeks.
**Decision:** One deployable service, with strict module boundaries inside it.
**Reason:** Three engineers cannot operate five services and still ship in eight weeks; modules give the same boundaries without the ops cost.
**Rejected:** Microservices — ops cost exceeds the team; separate frontend/backend repos — the wiring cost is not justified at this size.
**Consequences:** One deploy pipeline; if a module later needs independent scaling, the split is a deliberate, logged decision.
```
