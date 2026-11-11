# Simplicity and Anti-Overengineering Review

Read when auditing for unnecessary complexity or performing a `simplify` operation.

## Principle

Minimize **conceptual surface area and future change cost**, not merely line count. Favor plain, idiomatic code that addresses actual requirements. A simple function is good; a single obscure line is not. Never remove essential complexity merely to make a diff smaller.

## Detection checklist

Look for concrete examples, not arbitrary thresholds:

- **Tiny work wrapped in giant machinery:** a multi-class pipeline, factory, registry, or configurable strategy for a basic operation.
- **Excessive function size:** long functions that combine unrelated responsibilities or make control flow hard to follow; assess by cognitive load and invariants, not raw line count.
- **Pass-through layers:** wrappers that forward calls/objects without an important boundary, invariant, cross-cutting need, or test seam.
- **Premature abstraction:** generics/interfaces/plugins/helpers serving one simple call site with no credible second use or required extensibility.
- **Speculative generality:** config knobs, flags, fallback branches, migration bridges, and "future-proof" architecture with no current requirement.
- **Repeated trivial transformations:** unnecessary model mappings, DTO conversions, validation chains, adapters, serializers, or parsing.
- **Unnecessary state and side effects:** derived values stored and synchronized, global mutable state where local computation suffices, effects with avoidable dependencies.
- **Duplicate and dead code:** repeated meaningful rules, obsolete paths, unreachable flags, unused dependencies, stale feature switches, and redundant comments.
- **Overcomplicated tests/CI:** excessive mocks, fixtures, orchestration, fragile snapshots, and build steps for simple behavior.
- **Tooling cost:** extra services, databases, queues, packages, network hops, or code generators without a demonstrated benefit.

## Preferred simplification order

1. Delete genuinely unnecessary behavior or configuration (after confirming there are no users/callers).
2. Reuse a clear language/standard-library/framework feature already in the project.
3. Inline trivial pass-through abstractions when doing so reduces indirection without creating dangerous duplication.
4. Reduce unnecessary state, branches, and transformations while preserving semantics.
5. Split complicated functions along meaningful responsibilities **if that makes flow easier to understand**.
6. Consolidate duplicated *business rules* when consistency is important; do not create an elaborate helper merely to remove two obvious lines.
7. Remove a dependency/service/layer only when its remaining responsibilities can be met safely and with clear migration steps.

## Decision gate: simplify or leave alone?

Require all of these:

- **Evidence:** Show the specific cost today (confusing navigation, repetitive maintenance, extra dependency, difficult testing, or change risk).
- **Smaller mental model:** Fewer meaningful concepts or paths for the same current requirements.
- **Preserved contract:** Same observable behavior and needed nonfunctional constraints; account for errors, nulls, types, concurrency, authorization, i18n, and boundaries.
- **Economic value:** Expected benefit outweighs implementation/testing/migration risk. Do not refactor stable code for a subjective style preference.
- **Verification:** There is a proportionate way to show nothing important broke.

If one fails, propose/measure instead of rewriting.

## Tiny illustrative example

An unnecessary abstraction for addition:

```ts
class AdditionExecutor {
  execute(input: { left: number; right: number }): number {
    const operands = [input.left, input.right];
    return operands.reduce((acc, value) => acc + value, 0);
  }
}
```

If requirements only call for adding two numbers, prefer:

```ts
const add = (left: number, right: number): number => left + right;
```

This is **not** advice to remove input validation if the real API requires it, or to flatten complex financial rules into simple arithmetic. A direct expression only wins when it implements the same contract.

## Avoid anti-patterns in the simplifier itself

- Do not introduce an abstraction to "simplify" another abstraction without a net improvement.
- Do not change public APIs, database schemas, error types, or persistence representation for cosmetic brevity.
- Do not replace expressive names with cryptic abbreviations.
- Do not remove necessary layers for auditing, domain boundaries, access control, transaction consistency, or portability.
- Do not suggest arbitrary line-count ceilings, 100% abstraction reuse, or compulsory functional/object-oriented style.
- When no material simplification is available, say **"No worthwhile simplification found in the reviewed scope."**

## Evidence for a simplification proposal

Provide: location -> current complexity -> simpler alternative -> preserved invariants -> expected maintenance benefit -> risk/tests. Show a small before/after excerpt only if it clarifies the decision. Prefer fewer, higher-value suggestions over an exhaustive nitpick list.
