# Grooming

How to refine user stories and acceptance criteria from the technical team's perspective, surfacing every gap engineering needs closed before committing to the work.

## Two modes

Pick the mode first; the questions are the same, but who answers them differs.

| | Interview mode | Blast mode |
|---|---|---|
| Driver | The user captured the requirements themselves | A BA/PO owns the answers |
| Flow | One question at a time, user answers each | All questions written at once, with a recommendation each |
| After | Refine the US/AC from the answers | None: the BA answers in the meeting |

- **Interview mode** when the user wrote the stories themselves (e.g. straight from product discovery). Walk each branch until it resolves; do not move on until the user confirms. Recommend a default answer for each question so the user can accept it instead of writing prose.
- **Blast mode** when the stories came from a BA or PO. The question list goes to the grooming meeting, so every question carries a recommendation: the lean engineering answer, the default you would ship if forced to decide today, with the one-line consequence of the alternative. A recommendation turns the meeting into a review, not a blank interrogation.
- **Ambiguous:** ask once which mode fits, then run it.

Where a question can be answered by exploring the codebase, explore it first and fold the finding into the question (e.g. "The orders table has no `cancelled_at` column today: do you expect a soft-cancel or a hard delete?").

## The INVEST lenses

Build every question from these lenses. Each letter is a lens; omit a sub-point only if genuinely irrelevant. Lead with the questions that most threaten the estimate or feasibility.

- **I — Independent.** Can this story ship alone? What does it depend on? Does it hide a dependency story?
- **N — Negotiable.** What is genuinely flexible here, and what is fixed? Which part can be cut if the estimate is too big?
- **V — Valuable.** Who gets the value, and how will anyone know it worked? What is the observable outcome?
- **E — Estimable.** Is there enough detail to estimate? Which unknowns move the estimate most?
- **S — Small.** Can it fit in a sprint? If not, how does it slice into vertical, independently-shippable stories?
- **T — Testable.** Is every acceptance criterion verifiable without asking the author? Which unhappy paths are missing (validation, permission, empty states, errors)?

Phrase questions so a non-technical stakeholder can answer them. Translate technical risk into business consequence. Ask sharp, specific questions tied to *this* story; a question that could be asked of any story is usually too vague.

## Definition of Ready verdict

After the questions, a short verdict:

- **INVEST scorecard:** one line per letter, `PASS` / `WARN` / `FAIL`, with the single biggest gap for any letter that is not `PASS`.
- **Ready / Not ready for estimation:** and the one blocker that must close first if not ready.
- **Story-splitting suggestion:** if the story fails Small or Independent, propose how to slice it.
- **Acceptance-criteria gaps:** list any AC that is untestable, ambiguous, or missing, and propose concrete, verifiable wording.

## Refine (interview mode only)

Fold the decisions back into the source US/AC docs. Three change types, each as its own short list:

- **Add:** new US or AC the answers revealed (missing unhappy paths, permission variants, boundary cases, an unstated dependency story).
- **Remove:** US or AC the answers proved redundant, out of scope, or untestable beyond repair.
- **Edit:** reword an existing US or AC to match a decision, making it precise and testable.

Match the project's existing format and IDs. For an Add, use the next free ID; for Edit/Remove, cite the existing ID. Tie each applied change to the question/decision that drove it. In blast mode, never apply changes: the proposals live in the file for the BA to ratify.

## Where grooming lives

Write the session to a decision file (`docs/decisions/<YYYY-MM-DD>-grooming-<scope>.md`) so the work survives the session. The file holds the questions, the decisions, and the verdict. It sits with the other decision logs.
