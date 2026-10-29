---
applyTo: "**/*test*,**/*spec*,**/tests/**"
description: "Generic testing conventions for multi-layer software projects"
---

# Testing Conventions

## Test Strategy

- Use a balanced pyramid: unit, integration, and end-to-end tests.
- Add tests for every behavior change and bug fix.
- Keep tests deterministic and isolated.

## Backend Tests

- Unit-test business rules in services/domain logic.
- Integration-test controller + service + repository flows.
- Add contract tests for public interfaces/events when applicable.

## Frontend Tests

- Unit-test components and hooks with realistic user interactions.
- Integration-test page-level flows and interface-mocking boundaries.
- Add end-to-end tests for critical user journeys.

## Test Quality

- Use clear Arrange/Act/Assert structure.
- Prefer explicit expectations over snapshot-heavy tests.
- Avoid brittle timing dependencies.
