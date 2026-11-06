---
applyTo: "**/*.java"
description: "Java-specific performance add-on for the generic performance instructions. This file complements .github/instructions/performance.instructions.md and only adds Java-specific guidance not already covered there."
---

# Java Performance Add-on

> This file complements `.github/instructions/performance.instructions.md`.
> It only adds Java-specific guidance that is not already defined in the parent instructions.
>
> This file should be read together with `.github/instructions/java/development-rules.instructions.md` for Java-specific concurrency, memory safety, and data-structure decisions.

## Java data access rules

- Avoid N+1 query patterns and repeated repository calls inside loops.
- Prefer efficient bulk queries, joins, or batched fetches when a single operation can cover the required data.
- Keep repository methods focused and bounded to the needed result set.
- Use pagination for large result sets and large list/search operations.
- Do not fetch unnecessary entity graphs or large aggregate structures when a restricted DTO or projection is sufficient.

## Java transaction and memory rules

- Use read-only transactions for read-heavy operations.
- Avoid loading full entity graphs when only a subset is needed.
- Be explicit about cache scope and invalidation for frequently reused values.
- Release large objects and large collections promptly when they are no longer needed.
- Use bounded collections and safe eviction policies for caches and in-memory queues.
- Keep object lifecycles aligned with the memory safety rules defined in `.github/instructions/java/development-rules.instructions.md`.

## Java API performance rules

- Return only the data the caller needs; avoid sending deep object graphs or oversized payloads.
- Apply page limits and request-size constraints when appropriate.
- Avoid repeated expensive transformations in hot paths, especially inside loops.
- Prefer streaming or chunked processing for large outbound payloads when relevant.
- Keep serialization payloads compact and avoid leaking internal models through API responses.

## Java runtime guidance

- Prefer efficient collection types and bounded structures for large or frequently accessed data.
- Avoid repeated expensive work in loops or frequently called methods.
- Profile and measure before optimizing complex code paths.
- Keep repository and service methods predictable in cost so they scale with dataset size.
- Match the chosen collection type to the workload: use arrays for fixed-size primitives, lists for ordered dynamic data, maps for lookup-heavy logic, and concurrent structures for shared mutable state.

## Required Java performance checks

Before accepting a Java performance change, verify:

- no obvious N+1 or repeated scanning patterns remain
- the chosen data structures match the workload and concurrency model
- resource lifetime and caching behavior are bounded and safe
- expensive work is not happening in loops or per-item service calls without justification
- the implementation follows the Java development rules for memory safety and concurrency
