---
applyTo: "**/*"
description: "Generic code review checklist for multi-layer software projects"
---

# Code Review Checklist

## Architecture

- Respects frontend/backend separation.
- Preserves module boundaries for micro-UI/microservice domains.
- Respects layer direction: controller/interface → service → repository → model.

## Correctness

- Handles expected and edge-case behavior.
- Uses explicit error handling and meaningful failures.
- Keeps backward compatibility for existing contracts.

## Security

- Validates inputs and protects sensitive operations.
- Avoids secret leakage and unsafe logging.
- Applies authn/authz checks where required.

## Performance

- Avoids obvious N+1 or unbounded data reads.
- Uses pagination and efficient queries for list/search flows.
- Avoids unnecessary frontend rerenders and heavy bundles.

## Testing

- Includes tests proportional to risk and behavior changes.
- Keeps tests readable and deterministic.
