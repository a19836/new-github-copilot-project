---
applyTo: "**/*"
description: "Language-agnostic architecture guidance for hexagonal / ports-and-adapters design in multi-layer systems"
---

# Architecture Conventions

> This file is the shared, language-agnostic architecture baseline for new projects.
> It defines the default architectural model for AI-generated code across stacks and should be followed unless a language-specific rule explicitly overrides it.

## 1. Default architecture model

Use a hexagonal architecture (ports and adapters) by default for software projects that need maintainability, testability, and clear separation between business logic and infrastructure.

The model is:

- Domain / business logic in the center
- Application / use-case layer around it
- Adapters on the outside for transport, persistence, messaging, external APIs, and infrastructure

Core rule:

- Business logic must not depend on HTTP frameworks, database ORMs, UI frameworks, message brokers, or external SDKs.

## 2. Architectural principles

1. Keep business logic independent from infrastructure.
2. Put orchestration and use-case logic in an application/service layer.
3. Define explicit interfaces for external dependencies and behaviors.
4. Put framework-specific implementation details in adapters.
5. Keep dependencies pointing inward: infrastructure depends on application, application depends on domain.
6. Prefer small, explicit modules over broad abstractions.
7. Favor composition and explicit contracts over hidden framework magic.
8. Protect the domain from transport and persistence leakage.

## 3. Layer boundaries

Use a clear separation such as:

```text
interface / controller / transport
    ↓
application / service / use-case
    ↓
domain / model / business rules
```

And, for data access and external integrations:

```text
adapter / repository / gateway / client
    ↓
application port / interface
```

## 4. Domain and business rules

- Keep the domain free of UI, transport, and persistence details.
- Put business invariants in the domain or application layer, not in controllers or transport handlers.
- Do not let database entities, HTTP request objects, or UI models leak into the core business logic.
- Prefer explicit domain models and value objects over generic container objects.

## 5. Application layer

- Define use cases or service interfaces that describe business capabilities.
- Keep the application layer responsible for orchestration, validation, and business flow.
- Use ports to abstract persistence, messaging, external API access, and other infrastructure needs.
- Keep application logic independent from concrete frameworks.

## 6. Adapters and infrastructure

- Infrastructure code belongs outside the core business logic.
- Implement transport, persistence, and integration concerns in adapter modules.
- Keep adapters thin and focused on translation between external protocols and internal interfaces.
- Encapsulate framework-specific details in the adapter layer.

Examples of adapters:

- REST controllers or GraphQL handlers
- repository implementations
- database clients
- message producers/consumers
- external API clients
- file storage adapters

## 7. Data contracts and mapping

- Keep domain models separate from transport DTOs and persistence entities.
- Use explicit mapping between representations.
- Avoid exposing infrastructure objects directly to application or domain layers.
- Prefer clear, stable contracts over implicit conversions.

## 8. Validation and error handling

- Validate at trust boundaries.
- Return clear, stable application/domain errors rather than raw infrastructure errors.
- Do not expose internal implementation details, stack traces, or raw persistence errors to clients.
- Fail closed on unsafe or invalid state.

## 9. Dependency direction

The dependency direction must be inward.

```text
Infrastructure
    ↓
Application / Use Cases
    ↓
Domain / Business Rules
```

Forbidden patterns:

- Domain depending on transport frameworks
- Domain depending on persistence implementations
- Application depending directly on concrete repositories or external SDKs
- Controllers containing business logic
- Repositories embedding business rules

## 10. Testing strategy

- Test domain and use-case logic without the framework stack.
- Mock ports and adapters in application-level tests.
- Use infrastructure tests only for integration and adapter behavior.
- Prefer tests that validate business behavior rather than implementation details.

## 11. Lean implementation rules

- Prefer the smallest architecture that solves the real problem.
- Reuse existing patterns before introducing new abstractions.
- Avoid speculative layers, wrappers, or infrastructure abstractions that are not justified by current needs.
- Keep modules cohesive and aligned with the business domain.
- If a deliberate shortcut is accepted, document it clearly with `shortcut: <ceiling>, <upgrade path>`.

## 12. AI coding expectations

When creating or modifying code, AI agents should:

1. Identify the bounded context and business capability first.
2. Define the business rules and use cases before implementing infrastructure.
3. Create ports for required external dependencies.
4. Put framework-specific code in adapters, not in core logic.
5. Keep domain/application code independent from the runtime environment.
6. Validate that dependency direction remains inward before finalizing a change.

## 13. Project fit

This architecture is appropriate for:

- monoliths
- microservices
- layered backend applications
- frontend-backend separated systems
- framework-specific implementations such as Spring Boot, Express, NestJS, .NET, Python services, and others

The exact names and package conventions may vary by language, but the dependency direction and separation of concerns remain the same.
