# Human Handoff, Minimal Reports, and AI Independence

Read when writing a technical-health report, assessing AI knowledge debt, producing a project map, or documenting a change.

## Output language

The skill's instructions remain English. Write all user-facing **narrative and generated report prose in Spanish by default**, unless the user specifies otherwise. Preserve code, API names, commands, path names, and technical identifiers. Do not silently turn comments/docs in an English-language repo into Spanish; follow the project's existing documentation language unless the user explicitly requests Spanish docs.

## AI-independence test

Without relying on past agent conversation or private reasoning, locate evidence for six questions:

1. **Purpose and starting point:** What does the app do? How can a new developer start it locally?
2. **Architecture:** Which components exist and why? Where are the genuine boundaries?
3. **Data path:** Trace one core user/API flow through authorization, business rules, persistence, and response.
4. **Change points:** Where would someone add or modify a feature? Which contracts/invariants matter?
5. **Validation:** Which commands and tests verify the affected behavior? What environment is needed?
6. **Operations/recovery:** How is it configured/deployed and how could a failure be diagnosed or reversed?

Mark each `evidenced`, `partial`, or `unknown`. If unknown, cite the missing repository evidence and propose the shortest useful fix (one paragraph, a test, a brief ADR, or a README correction). Do not automatically create large diagrams, encyclopedic walkthroughs, or claims about the system beyond the inspected scope.

## Report structure (translate headings to the user's language)

Aim for one page, approximately 250–500 words, **not** a mandatory word count. If urgent issues exceed this, include them rather than hiding them. Prefer these sections in order:

```markdown
# Technical Health — <project or module>
Scope: <files/flows examined> | Mode/depth: <...> | Date: <...>

## How it works (one minute)
<3–5 human-readable sentences, with paths to entry points>

## Overall status
<What is verified; important strengths; no invented grades>

## Top findings and simplifications
| Priority | Type / confidence | Problem and practical cost | Evidence | Smallest safe action / how to verify |
| --- | --- | --- | --- | --- |
<3–5 high-value items; all P0s regardless of count>

## Limitations and unverified assumptions
<Up to 3–5 important unknowns, clearly labeled>

## Changed and checked
<Exact files/actions/tests and passed/failed/not run results, or "No code changes">

## Next action
<One highest-return recommendation; say if approval is needed>
```

For `explain`, favor a short flow map and answers to the AI-independence questions over a large list of smells. For `close`, focus on the diff and the debt ratchet. For `simplify`, explain **before -> after -> preserved contract** and verify that complexity decreased, not just LOC.

## Project map when explicitly requested

Prefer an existing README/architecture doc. Otherwise use `docs/PROJECT_MAP.md`, ideally <= 60 lines:

- Purpose and deployment boundary
- Key components, entry points, and where the truth lives
- One or two core request/data flows, with authorization and storage
- Where to change behavior and run relevant tests
- Important invariants, failure/recovery points, known unknowns
- Links to existing docs and code; no copied manuals

## Technical health file when explicitly requested

Prefer an existing issue tracker or project health document. Otherwise use `docs/TECH_HEALTH.md`, ideally <= 90 lines:

- Scope/date of most recent assessment
- Top confirmed problems and material suspected risks; stable issue IDs if follow-up is expected
- Type (`debt` / `defect` / `vulnerability` / `limitation` / `trade-off` / `improvement`), confidence, priority, evidence, decision
- Current remediation status and verification references
- Known limitations and next high-value action

When an issue is fixed, update its status; do not duplicate whole dated reports. Avoid reporting large piles of low-value style observations. Keep risk acceptance explicit, with reason and revisit trigger. Refer to existing ADRs for consequential choices; write a new ADR only when a significant decision actually needs preservation.

## Honest scope language

Use statements like:

- "Reviewed these modules and representative flows; other components were not assessed."
- "Likely issue; confirmation requires an integration test under the real database."
- "Simplification proposed, not applied."
- "Implemented but not verified because CI/dependencies were unavailable."
- "No material debt found in the reviewed diff" rather than "The project has no technical debt."
