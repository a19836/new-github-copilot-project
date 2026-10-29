---
applyTo: "**/*.py"
description: "Python-specific security add-on for the generic security instructions. This file complements .github/instructions/security.instructions.md and only adds Python-specific guidance not already covered there."
---

# Python Security Add-on

> This file complements `.github/instructions/security.instructions.md`.
> It only adds Python-specific guidance that is not already defined in the parent instructions.

## Python input validation

- Validate all inbound data at the application boundary.
- Reject malformed, missing, or unexpected values early.
- Use schema validation or explicit validators before business logic executes.

## Python authorization guidance

- Enforce authentication for protected actions.
- Apply authorization checks at both the API boundary and the service layer when needed.
- Prefer least privilege and explicit permission checks over permissive defaults.

## Python secret and data handling

- Never hardcode credentials, tokens, or private keys in source files.
- Read secrets from environment variables or a secure secret manager.
- Do not expose stack traces, internal exception details, or sensitive data to external callers.
- Sanitize sensitive values before logging or metrics output.

## Python persistence safety

- Use parameterized queries and safe ORM or database patterns.
- Never interpolate untrusted values directly into SQL or command strings.
- Validate identifiers and incoming payloads before persistence access or downstream execution.

## Defensive defaults for Python apps

- Fail closed on invalid state rather than silently accepting unsafe input.
- Prefer explicit validation and clear exceptions over permissive fallbacks.
- Keep access control, error handling, and data validation intentional and predictable.
