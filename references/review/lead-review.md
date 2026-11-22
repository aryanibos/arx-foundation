# Lead Review

How to review a pull request as the tech lead: against the project's own standards, with evidence, in rounds, until the findings are verified resolved.

## What backs a finding sets its severity

| Severity | Backed by | Can the author argue? |
|---|---|---|
| **BLOCKER** | Proof (a reproduction you ran, or a `file:line` trace), or a written project rule quoted verbatim | No |
| **NIT** | Neither. It is your preference. | Yes. The author may decline it with a reason. |
| **QUESTION** | A suspicion that only the author can settle | Not yet. The answer decides. |

Written rules are the checklist, the coding standard, the instruction files, specs, decision logs, AC THEN clauses, and the PR template. A finding with a Rule or Proof must be a BLOCKER; a NIT has neither. Never remove a Rule or Proof to lower a severity. A NIT you raise PR after PR belongs in a written rule: write the rule, or stop raising it.

## The review

1. **Read the project's standards first.** README, instruction files, coding standard, review checklist, and the specs and AC the PR claims to satisfy.
2. **Check out the change in isolation** (a worktree, never the author's working copy). Run the project's verification commands: type-check, lint, format, test, build. Record the gate line honestly; a gate that did not run is reported as not run, never as passed.
3. **Walk the checklist.** Every item, in order. For each item, the evidence is a file and line, or the item is marked as checked with nothing to report.
4. **Prove each finding.** A finding without proof is a suspicion, not a finding. Reproduce the bug, or trace the exact lines that violate the quoted rule. Quote the rule verbatim.
5. **Post the review in a fixed format, identical across rounds.** Same shape every round: gate line, checklist walk, findings (each with ID, severity, rule or proof, file:line, and what would resolve it), verdict.
6. **Verdict:** Approve, Request Changes, or Comment. Approve only the exact commit that was reviewed — an approval never carries forward to new commits.

## Re-review (rounds)

- Findings keep stable IDs across rounds. A finding is never considered resolved only because a commit message says it was fixed: resolution must be verified in code, tests, documentation, migration evidence, CI, or another appropriate source.
- Each round verifies the previous findings, reviews the delta, and detects regressions or new findings.
- If the author declines a NIT with a reason, record the reason and move on. If the author disputes a BLOCKER, the rule or the proof decides — not the louder voice.

## Reviewing without a PR

The same steps apply to commits that reached a branch directly: review a range from a given commit, or one author's commits in a branch, with the rounds kept in an issue. The rules do not change because the ceremony is missing.

## What review never does

- Never approves a commit that was not reviewed.
- Never lets a finding disappear by being deleted from the review.
- Never treats "the author is senior" as evidence. Evidence is code, tests, and quoted rules.
- Never reviews against taste. If a preference keeps appearing, write it as a rule first, then enforce it.
