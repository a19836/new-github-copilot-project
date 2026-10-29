# AGENTS.md

> Generic starter for projects with separated frontend/backend, micro-UIs, microservices, and layered modules.

## Target Architecture

- **Frontend** is split into **micro-UIs** by business domain.
- **Backend** is split into **microservices** by bounded context.
- Each module uses clear layers: **controller/interface → service/use-case → repository/data-access → model/domain**.

## Recommended Top-Level Structure

```text
frontend/
backend/
shared-contracts/      # optional: schemas/types shared by front and back
infra/                 # optional: deployment and environment assets
```

## Module Structure (per microservice or micro-UI)

```text
modules/<domain>/
  models/
  repositories/
  services/
  controllers/         # backend
  dtos/|contracts/     # backend
  interface/           # frontend interface adapters (for micro-UIs)
  components/          # frontend UI components
  tests/
```

## Copilot Instruction Map

| Domain | File |
|---|---|
| Global project rules | [.github/copilot-instructions.md](.github/copilot-instructions.md) |
| Backend layering and interface rules | [.github/instructions/backend.instructions.md](.github/instructions/backend.instructions.md) |
| Frontend and micro-UI rules | [.github/instructions/frontend.instructions.md](.github/instructions/frontend.instructions.md) |
| Database and migration rules | [.github/instructions/database.instructions.md](.github/instructions/database.instructions.md) |
| Security rules | [.github/instructions/security.instructions.md](.github/instructions/security.instructions.md) |
| Performance rules | [.github/instructions/performance.instructions.md](.github/instructions/performance.instructions.md) |
| Testing rules | [.github/instructions/testing.instructions.md](.github/instructions/testing.instructions.md) |
| Code review checklist | [.github/instructions/code-review.instructions.md](.github/instructions/code-review.instructions.md) |
| Java add-ons | [.github/instructions/java](.github/instructions/java) |
| React add-ons | [.github/instructions/react](.github/instructions/react) |
| Python backend add-ons | [.github/instructions/python](.github/instructions/python) |

## Specialized Agents

| Agent | Use for |
|---|---|
| [Code Reviewer](.github/agents/code-reviewer.agent.md) | General review and architecture/layer checks |
| [Backend Reviewer](.github/agents/backend-reviewer.agent.md) | Interface design, service boundaries, repository usage |
| [Frontend Reviewer](.github/agents/frontend-reviewer.agent.md) | Micro-UI structure, component patterns, state and UX |
| [Security Reviewer](.github/agents/security-reviewer.agent.md) | AuthN/AuthZ, input validation, data exposure |
| [Performance Reviewer](.github/agents/performance-reviewer.agent.md) | Query efficiency, caching, rendering and latency |
| [Test Specialist](.github/agents/test-specialist.agent.md) | Unit/integration/contract/e2e tests |
| Java-specific review add-ons | [.github/agents/java](.github/agents/java) |
| React-specific review add-ons | [.github/agents/react](.github/agents/react) |
| Python-specific review add-ons | [.github/agents/python](.github/agents/python) |

Language-specific folders are additive only and should extend the generic rules without repeating them.

## Default Review Flow

1. Run **Code Reviewer** first.
2. Add specialists depending on changed files and risks.
3. Require tests for new features or behavior changes.
