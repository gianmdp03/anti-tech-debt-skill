# Safe Simplification, Fixes, Ratchet, and Recovery

Read for `simplify`, `fix`, `close`, or `recovery` operations.

## Authorization matrix

- **Audit/explain:** inspect and report; do not modify code. Only write docs if asked.
- **Simplify/fix:** modify code in the explicitly authorized area; preserve public behavior unless change is explicitly requested.
- **Close:** check change-scope for new debt and regressions; only make corrections that belong to the already authorized coding task.
- **High-risk:** schema/data loss, live systems, deploys, multi-tenant permissions, payment/clinical records, large redesign, or destructive file operations require explicit approval and a rollback strategy.

## Change sequence

1. **Baseline.** `git status`, diff, instructions, scope, environment; identify tests and relevant contracts. Avoid overwriting uncommitted changes.
2. **Claim and proof.** State the demonstrated problem (or uncertainty), impacted behavior, expected smallest fix, and success criteria. A "simpler" proposal must actually reduce concepts/maintenance load.
3. **Invariants.** List must-preserve properties: outputs, errors, validation, permissions, concurrent semantics, data representation, APIs, UI states, and supported platforms.
4. **Tests first when useful.** Add/identify a characterization or contract test covering the key invariant. For a known bug, attempt reproduction. If baseline is already failing, record it rather than attributing it to the patch.
5. **Minimal edit.** One coherent change per logical patch. Favor deletion or local restructuring over introducing new dependencies or architecture. Respect existing style and patterns.
6. **Verify.** Run smallest relevant tests plus integration/build/type/security checks dictated by impact. For changed permissions or data, include negative and cross-tenant tests. For performance, compare like-for-like measurements.
7. **Inspect.** Review diff and git status; check unrelated files, generated artifacts, user edits, dead references, and changes in exposed contracts. Plan rollback for any nontrivial risk.
8. **Report truthfully.** `verified` only when relevant checks actually passed. Otherwise state `implemented, not verified` and list blockers. Do not call `skipped` a success.

## Debt ratchet (changed scope only)

Compare before and after:

| Dimension | Evidence to check |
| --- | --- |
| Comprehension | Can a new maintainer trace and change the flow? |
| Complexity | Did concepts, branches, wrappers, dependencies, or manual steps decrease? |
| Contracts | Are APIs, data, permissions, and key invariants preserved? |
| Tests | Did meaningful protection improve or degrade? |
| Operational risk | Did recovery, observability, reliability, or deployment become harder? |
| Residual debt | Was risk removed, merely moved, or deliberately accepted? |

No pseudo-precise universal health score. If an important new P0/P1 emerges, do not claim successful closure; explain and remedy in scope or escalate explicitly. Legitimate design trade-offs may add complexity when they buy necessary behavior or safety.

## Incident/recovery-specific rules

- Avoid speculative broad refactors or destructive diagnostics.
- Isolate the exact failure boundary; preserve evidence and user data.
- Prefer mitigations that can be reversed; clarify if urgent containment requires permission.
- Characterize the failed contract before and after correction when feasible.
- Record root cause vs symptom; add a regression test or lightweight detection for repeat incidents.

## Example completion statement

"Reviewed auth boundary and two API handlers. Removed an unnecessary pass-through service while preserving authorization checks. Targeted tests: 18 passed; integration tests: not run (database unavailable). New debt: none observed in the reviewed diff; whole-repository health not assessed."
