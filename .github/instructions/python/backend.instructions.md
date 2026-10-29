---
applyTo: "**/*.py"
description: "Python-specific backend add-on for the generic backend instructions. This file complements .github/instructions/backend.instructions.md and only adds Python-specific guidance not already covered there."
---

# Python Backend Add-on

> This file complements `.github/instructions/backend.instructions.md`.
> It only adds Python-specific guidance that is not already defined in the parent instructions.

## Python layering patterns

- Keep API or transport code focused on request handling and validation.
- Put business logic in service or use-case functions/classes.
- Keep persistence or data-access concerns in repository or adapter layers.
- Keep domain invariants and validation rules in domain models or service logic.
- Prefer constructor-based dependency injection or explicit dependency wiring in application startup code.

## Python project structure

Use clear package organization such as:

```text
app/
  api/
  services/
  repositories/
  models/
  schemas/
  core/
  config/
  exceptions/
```

Keep package boundaries explicit and avoid mixing HTTP, persistence, and business logic in the same module.

## Python API and schema guidance

- Prefer explicit request/response schemas or DTOs instead of exposing persistence models directly.
- Validate inputs at the boundary and reject malformed or missing values early.
- Keep API contracts stable and intentional as the application evolves.
- Translate persistence errors or domain rules into clear application exceptions at the service boundary.
- Keep transport concerns separate from domain logic.

## Python dependency and transaction guidance

- Keep transaction or session boundaries in the service layer when the framework supports them.
- Use read-only or non-mutating patterns for read-heavy operations when available.
- Keep configuration, framework wiring, and business logic separate.
- Prefer explicit dependency injection or factory-based composition over hidden global state.

## Python design practices

- Favor small, focused modules and functions with clear responsibilities.
- Avoid mixing transport, persistence, and business logic in one file or class.
- Fail fast on invalid state and reject malformed input as early as possible.
- Keep exceptions meaningful, actionable, and aligned with business semantics.
