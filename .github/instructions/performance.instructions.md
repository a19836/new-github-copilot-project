---
applyTo: "**/*"
description: "Generic performance conventions for distributed systems and presentation layers"
---

# Performance Conventions

## Backend

- Avoid N+1 query patterns; batch or prefetch intentionally.
- Use pagination for list endpoints by default.
- Select only required fields for read-heavy endpoints.
- Add indexes for high-frequency filters, joins, and sorts.
- Prefer async/background processing for slow non-critical work.

## Frontend

- Lazy-load routes and heavy UI modules.
- Avoid unnecessary re-renders; keep state close to where it is used.
- Debounce/throttle high-frequency user-driven requests.
- Use caching/revalidation patterns for remote data.

## Cross-Cutting

- Define latency and throughput expectations per endpoint/use-case.
- Measure before and after non-trivial optimizations.
