---
applyTo: "**/*"
description: "Generic backend conventions for layered modules and distributed services"
---

# Backend Conventions

## Layering Rules (mandatory)

- `controller`/`interface` layer depends on `service` only.
- `service` layer contains business logic and orchestration.
- `repository`/`data-access` layer handles persistence and external data sources.
- `model`/`domain` layer contains entities, value objects, and domain rules.
- Never call repositories directly from controllers.

## Module Structure

Use domain modules such as:

```text
services/<service-name>/modules/<domain>/
  models/
  repositories/
  services/
  controllers/
  contracts/
  tests/
```

## Interface Rules

- Keep exposed interfaces resource-oriented and versioned.
- Validate request inputs at the boundary.
- Return DTOs/contracts, never raw persistence entities.
- Use explicit error responses with stable error codes.

## Service Rules

- Keep methods focused on use-cases, not CRUD wrappers only.
- Enforce business invariants in services/domain, not in controllers.
- Use transactions where consistency is required.

## Repository Rules

- Keep repositories free of business rules.
- Avoid leaking query/persistence details outside service layer.
- Prefer pagination and filtering over unbounded `findAll`.

## Microservice Boundaries

- One service owns one bounded context and its data model.
- Cross-service access happens through interfaces/events, never direct database access.
- Keep contracts backward compatible when possible.
