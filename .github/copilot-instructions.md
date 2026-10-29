# Copilot Instructions (Generic Starter)

This repository is a reusable starter template for modern web projects with:

- **Frontend/Backend separation**
- **Micro-UIs** in the frontend
- **Microservices** in the backend
- **Layered module architecture** in every domain

## Core Architectural Rules

1. Keep frontend and backend in separate applications/deployments.
2. Organize by **business domain**, not by technical type at root level.
3. In each domain module, enforce layer boundaries:
   - `controller`/`interface` (transport)
   - `service`/`use-case` (business logic)
   - `repository`/`gateway` (data access/integration)
   - `model`/`domain` (core entities and value objects)
4. Controllers must never access repositories directly.
5. Shared contracts (schemas/types) should be versioned and explicit.

## Default Structure

```text
frontend/
  apps/ or shell/
  micro-uis/<domain>/
backend/
  services/<domain-service>/
shared-contracts/     # optional
infra/                # optional
```

## Microservice Guidance

- One microservice per bounded context.
- Prefer independent data ownership per service.
- Communicate through stable interfaces/events with versioning.
- Keep internal models private; expose DTOs/contracts only.

## Micro-UI Guidance

- One micro-UI per bounded context/capability.
- Keep each micro-UI deployable or independently releasable when feasible.
- Use a shell/composition layer for routing, auth context, and shared design system.

## Development Expectations

- Follow existing project scripts for build, lint, type-check, and tests.
- Add or update tests for behavior changes.
- Keep changes small, cohesive, and reversible.
- Prefer secure defaults and explicit validation.

## Commit Convention

Use Conventional Commits:

```text
type(scope?): description
```

Examples:

- `feat(auth): add refresh token flow`
- `fix(cart): prevent duplicate checkout requests`

## Instruction Files

- Backend rules: `.github/instructions/backend.instructions.md`
- Frontend rules: `.github/instructions/frontend.instructions.md`
- Database rules: `.github/instructions/database.instructions.md`
- Security rules: `.github/instructions/security.instructions.md`
- Performance rules: `.github/instructions/performance.instructions.md`
- Testing rules: `.github/instructions/testing.instructions.md`
- Review checklist: `.github/instructions/code-review.instructions.md`
- Java add-ons: `.github/instructions/java/`
- React add-ons: `.github/instructions/react/`
- Python backend add-ons: `.github/instructions/python/`

Language-specific folders are additive only: they extend the generic rules without repeating them.
