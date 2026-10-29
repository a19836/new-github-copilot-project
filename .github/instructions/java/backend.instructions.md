---
applyTo: "**/*.java"
description: "Java-specific backend add-on for the generic backend instructions. This file complements .github/instructions/backend.instructions.md and only adds Java-specific guidance not already covered there."
---

# Java Backend Add-on

> This file complements `.github/instructions/backend.instructions.md`.
> It only adds Java-specific guidance that is not already defined in the parent instructions.

## Java-specific layering patterns

- Keep endpoint/controller classes focused on HTTP transport concerns only.
- Put business logic in service or use-case classes.
- Keep persistence concerns inside repositories or data-access components.
- Keep domain invariants in model/domain objects and service-layer validation.
- Prefer constructor-based dependency injection and avoid field injection in new code.

## Java package and module conventions

Use explicit, layered package organization such as:

```text
com.example.app
  controller/
  service/
  repository/
  model/
  dto/
  mapper/
  config/
  exception/
```

Keep package boundaries aligned with the modular architecture and avoid mixing transport, persistence, and business logic in the same package.

## Java API and DTO guidance

- Prefer DTOs or explicit request/response contracts instead of exposing persistence entities.
- Use immutable DTOs when possible, especially for request/response records.
- Keep API contracts explicit and backward-compatible when evolving models.
- Translate domain or persistence exceptions into clear application exceptions at the boundary.
- Validate the input at the API boundary and re-validate critical invariants in the service layer when needed.

## Java transaction and configuration guidance

- Keep transaction boundaries in the service layer.
- Use read-only transactions for read-heavy operations.
- Keep configuration, infrastructure wiring, and business logic separated.
- Prefer explicit dependency injection over hidden framework lookups.

## Java design practices

- Favor small, focused classes with a single responsibility.
- Split responsibilities across layers instead of combining transport, persistence, and business logic in one class.
- Fail fast on invalid state and reject malformed input early.
- Keep application exceptions meaningful, actionable, and stable across layers.
