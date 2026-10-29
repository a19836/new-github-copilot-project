---
applyTo: "**/*.py"
description: "Python-specific performance add-on for the generic performance instructions. This file complements .github/instructions/performance.instructions.md and only adds Python-specific guidance not already covered there."
---

# Python Performance Add-on

> This file complements `.github/instructions/performance.instructions.md`.
> It only adds Python-specific guidance that is not already defined in the parent instructions.

## Python data access rules

- Avoid N+1 query patterns and repeated database calls inside loops.
- Prefer bulk queries, joins, or batched access when one operation can cover the required data.
- Keep repository and query functions focused and bounded to the needed result set.
- Use pagination for large result sets and list/search operations.

## Python memory and I/O rules

- Use read-only or streaming patterns where possible for large data reads.
- Avoid loading full datasets into memory when only a subset is required.
- Be explicit about cache scope and invalidation for frequently reused values.
- Release large objects promptly when they are no longer needed.

## Python API performance rules

- Return only the data the caller needs; avoid oversized payloads and deep object graphs.
- Apply request-size and page limits when appropriate.
- Avoid repeated expensive transformations in hot paths, especially inside loops.
- Prefer streaming or chunked processing for large outbound payloads when relevant.

## Python runtime guidance

- Prefer efficient collection types and bounded structures for large or frequently accessed data.
- Avoid repeated expensive work in loops or frequently called functions.
- Profile and measure before optimizing complex code paths.
- Keep repository and service methods predictable in cost so they scale with dataset size.
