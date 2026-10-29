---
applyTo: "**/*"
description: "Generic security conventions for multi-layer software projects"
---

# Security Conventions

## Authentication and Authorization

- Enforce authentication for protected interfaces.
- Enforce authorization at interface boundaries and sensitive service operations.
- Apply least privilege in role/permission design.

## Input and Output Safety

- Validate all external inputs (request body, params, headers, events).
- Encode/escape output in UI contexts to prevent XSS.
- Never expose internal errors, stack traces, or sensitive internals to clients.

## Secrets and Credentials

- Never hardcode secrets, tokens, passwords, or private keys.
- Store secrets in environment variables or a secrets manager.
- Rotate credentials and use scoped access keys.

## Service-to-Service Security

- Use authenticated service-to-service communication.
- Validate scopes/claims for internal interfaces.
- Apply rate limiting and audit logging on sensitive endpoints.
