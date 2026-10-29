---
applyTo: "**/db/**,**/migrations/**,**/*Repository.*,**/*Model.*,**/*Entity.*"
description: "Generic database conventions for microservices and layered modules"
---

# Database Conventions

## Ownership and Boundaries

- Prefer **database per microservice** (or strict schema ownership).
- Never let one service write directly into another service's tables.
- Treat schema changes as versioned contracts.

## Migrations

- Use migration files for all schema changes.
- Make migrations deterministic and idempotent where possible.
- Include rollback guidance when your toolchain supports it.

## Schema and Query Design

- Use stable primary keys and explicit foreign keys.
- Index columns used for joins, filters, and sorting.
- Keep indexes intentional; remove unused/heavy indexes.
- Avoid N+1 access patterns from ORM/data mappers.
- Paginate large list queries by default.

## Data Safety

- Do not drop/rename critical columns without a migration plan.
- Backfill in controlled batches for large datasets.
- Validate constraints before enforcing `NOT NULL` on existing data.
