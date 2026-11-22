# Requirements Format

The format for user stories and acceptance criteria, and the traceability rules that keep them honest.

## User stories

One row per story:

```text
| ID    | Persona  | Action          | Business value   |
| US-01 | <role>   | <what they do>  | <why it matters> |
```

Rules:

- One `US-XX` per actor goal. Number from `US-01`; preserve IDs already present in the source document rather than renumbering.
- Trace every story to a persona and a stated job. Do not invent scope the user did not describe; if something is needed but unconfirmed, list it as an open question, not a story.
- Preserve the user's domain terms and any non-English labels verbatim.

## Acceptance criteria

One criterion per observable outcome, tied to its story, written as a Gherkin scenario:

```gherkin
AC-01.01: <short scenario name>
  Given <context>
  When <action>
  Then <observable result>
```

Rules:

- Number criteria `AC-<story>.<seq>` (`AC-01.01`). Keep IDs stable.
- One scenario per AC. Do not merge or split.
- Keep Gherkin steps in canonical order: all `Given`/`And` context, then `When`, then `Then`. Multiple whens/thens use `And`.
- Every criterion is verifiable without asking the author. If an AC is vague, render it as-is and flag it in a trailing note rather than fixing it silently (fixing is grooming's job).
- Cover the happy path and the obvious unhappy paths (validation, permission, empty states) for each story. Thin AC now means a weak grooming pass later.

## Sprint breakdown

Group stories into sprints with a one-line goal each. Sequence so foundational stories (auth, core entity creation) land first and dependent stories follow. Never invent a sprint grouping: if the source document has none, ask the user for it.

## Traceability

- Every story traces to a persona and a job from the source document.
- Every criterion traces to its story.
- Every task card traces to the AC IDs it satisfies.
- Every API operation traces to the AC/US that justifies it.
- An item tracing to nothing is a flag to raise, not a row to write.

## Writing conventions

- No filler or hedging; every sentence informs.
- No em-dashes, no double-dashes (`--`) in prose; dashes only as Markdown syntax or in literal code/CLI flags.
- No emoji. Professional, declarative tone.
- If a document carries a metadata header (`**Version:**`, `**Date:**`, `**Author:**`, `**Status:**`, `**Phase:**`), each such line ends with two trailing spaces so Markdown renders them on separate lines.
