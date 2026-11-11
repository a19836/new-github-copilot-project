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
- Before writing code, identify the actual scope: affected callers, tests, configs, and data flows.
- Prefer the smallest solution that fully works; avoid feature creep and speculative abstractions.
- Reuse existing project patterns, helpers, and platform capabilities before adding custom wrappers, dependencies, or boilerplate.
- Do not cut safety, validation, security, or user-visible correctness in the name of being “minimal.”
- When a deliberate compromise is accepted, document it with a clear `shortcut: <ceiling>, <upgrade path>` comment so the tradeoff is visible and reviewable.

## Lean Delivery and YAGNI

- Prefer the smallest change that fully solves the task.
- Reuse existing project patterns, helpers, and platform features before introducing abstractions, custom wrappers, or new dependencies.
- Avoid speculative features, bloated configuration, and "for later" code that is not required by the current task.
- Keep values and interfaces aligned with the platform or project conventions already in place.
- Keep the implementation understandable to a busy human in one read.
- Prefer deletion and simplification over adding extra code.

## Execution and review mindset

- Read the task and the code it touches before writing.
- Trace the real behavior end-to-end instead of only looking at the local diff.
- A change can break code it does not directly edit; check callers and shared behavior before finalizing.
- Favor correctness, safety, and maintainability over style preferences or unnecessary optimization.
- If a shortcut is intentional, mark it with a clear limit and a revisit path.

## Commit Convention

Use Conventional Commits:

```text
type(scope?): description
```

Examples:

- `feat(auth): add refresh token flow`
- `fix(cart): prevent duplicate checkout requests`

## Instruction Files

- Architecture baseline: `.github/instructions/architecture.instructions.md`
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
