---
applyTo: "**/*.java"
description: "Java-specific security add-on for the generic security instructions. This file complements .github/instructions/security.instructions.md and only adds Java-specific guidance not already covered there."
---

# Java Security Add-on

> This file complements `.github/instructions/security.instructions.md`.
> It only adds Java-specific guidance that is not already defined in the parent instructions.

## Java input validation

- Validate all inbound data at the application boundary.
- Reject malformed, missing, or unexpected values early.
- Apply Bean Validation or equivalent validation constraints before business logic executes.

## Java authorization guidance

- Enforce authentication for protected actions.
- Apply authorization checks at both the interface boundary and the service layer when needed.
- Prefer least privilege and explicit permission checks over broad access.

## Spring Boot security standard for new Java projects

- When the project is a new Spring Boot Java application, implement authentication and authorization using the default Spring Security framework conventions.
- Prefer the standard Spring Boot security model based on `SecurityFilterChain`, `UserDetailsService`, `PasswordEncoder`, and role/authority-based authorization.
- Override the framework defaults with custom configuration classes when needed, following the standard Spring Boot tutorials and idioms rather than creating a completely custom security layer.
- Use the default Spring Security context and request filtering mechanisms for session/JWT-based flows when appropriate, while keeping the implementation aligned with the official Spring Boot patterns.
- Apply this rule ONLY TO NEW Spring Boot Java projects; other Java projects should continue to follow the generic security guidance in this repository.

## Java secret and data handling

- Never hardcode credentials, secrets, tokens, or private keys in source files.
- Use environment variables or a secure secret manager for runtime configuration.
- Do not expose stack traces, internal exception details, or sensitive data to clients.
- Sanitize sensitive values before logging or metrics output.

## Java persistence safety

- Use parameterized queries and safe repository patterns.
- Never interpolate untrusted values directly into query strings.
- Validate identifiers and incoming payloads before persistence access or downstream calls.

## Defensive defaults for Java apps

- Fail closed on invalid state rather than silently accepting unsafe input.
- Prefer explicit validation and clear exceptions over permissive fallback behavior.
- Keep access control, error handling, and data validation intentional and predictable.
