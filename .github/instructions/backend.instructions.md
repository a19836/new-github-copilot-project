---
applyTo: "**/*"
description: "Generic backend conventions for layered modules and distributed services"
---

# Backend Conventions

> This file should be read together with `.github/instructions/architecture.instructions.md` for the language-agnostic hexagonal architecture baseline and `.github/instructions/java/architecture.instructions.md` when the project is a Java/Spring Boot service.

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

## Simplicity and minimal-change rules

- Prefer the smallest backend change that fully solves the user problem.
- Reuse existing domain services, patterns, and framework conventions before adding new abstractions or extra layers.
- Avoid speculative features, optional configuration, or wrapper code that does not directly support the current use case.
- Reuse established framework or standard library capabilities before introducing custom implementations.
- If a short-lived compromise is required, document it with `shortcut: <ceiling>, <upgrade path>` so it remains explicit and reviewable.

## Architecture baseline

- Follow `.github/instructions/architecture.instructions.md` as the default language-agnostic architecture standard.
- For Java/Spring Boot projects, also follow `.github/instructions/java/architecture.instructions.md` for the concrete hexagonal architecture expectations.
- Keep the application/service layer independent from transport and persistence implementations.

## Microservice Boundaries

- One service owns one bounded context and its data model.
- Cross-service access happens through interfaces/events, never direct database access.
- Keep contracts backward compatible when possible.
