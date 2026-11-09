---
applyTo: "**/*"
description: "Generic code review checklist for multi-layer software projects"
---

# Code Review Checklist

## Review workflow

- Review the user-named scope first: uncommitted changes, staged files, a branch, a PR, or a specific folder.
- Read the diff first, then the code it touches: callers of changed functions, the functions it calls, the tests, and the relevant documentation.
- Trace the real flow end-to-end: where data enters, what is stored, and what leaves the system.
- A change can break code it does not directly edit; check impacted callers and shared functions before concluding the fix is safe.
- For expected load, assess whether the code is designed for one user or many users/processes, and call out risk assumptions when relevant.

## Architecture

- Respects frontend/backend separation.
- Preserves module boundaries for micro-UI/microservice domains.
- Respects layer direction: controller/interface → service → repository → model.
- Avoids unnecessary layers, wrappers, or abstractions that do not solve a real problem.
- Keeps responsibilities aligned with the existing project structure and bounded contexts.

## Correctness

- Handles expected and edge-case behavior.
- Uses explicit error handling and meaningful failures.
- Keeps backward compatibility for existing contracts.
- Checks for mis-sized boundaries, zero/empty values, last-item behavior, time-zone issues, and other edge cases that commonly break production systems.
- Identifies when a fix was applied in one caller but the shared function remains broken.

## Security

- Validates inputs and protects sensitive operations.
- Avoids secret leakage and unsafe logging.
- Applies authn/authz checks where required.
- Looks for injection risks, weak randomness, unsafe data handling, missing validation at trust boundaries, and insecure defaults.
- Ensures data-loss risks are not hidden by swallowed errors or unsafe write ordering.

## Performance

- Avoids obvious N+1 or unbounded data reads.
- Uses pagination and efficient queries for list/search flows.
- Avoids unnecessary frontend rerenders and heavy bundles.
- Identifies repeated work, per-item loops, unbounded collections, and heavy operations that hurt scale.
- Evaluates whether the design is appropriate for the expected load and whether the system will hold under realistic concurrency.

## Testing

- Includes tests proportional to risk and behavior changes.
- Keeps tests readable and deterministic.
- Requires a concrete test for risky logic, bug fixes, security-sensitive behavior, data writes, and edge-case handling.
- Checks whether failure paths and regression cases are covered, not just happy paths.

## Lean and scope discipline

- Prefer the smallest change that fully solves the task.
- Flag speculative features, duplicate logic, and unnecessary abstractions.
- Prefer reuse of existing project helpers, standards, and platform capabilities before adding new dependencies or wrappers.
- If a project accepts a deliberate shortcut, confirm it is documented with a clear `shortcut: <ceiling>, <upgrade path>` comment and that the tradeoff is understood.
- Do not review only the diff; review the actual impact on callers, tests, config, and data flows.
- Prefer deleting or simplifying code over adding layers, frameworks, or configuration the problem does not require.

## Review output expectations

- Findings should be concrete and tied to a real condition or scenario.
- Prefer the smallest fix that addresses the root cause.
- Report issues in priority order: correctness, security, data integrity, scale, testing, speed, and then lean/cleanup.
- Keep the language simple and actionable; if a technical concept is used, explain it in plain terms the first time.
- Do not report style-only comments unless they materially affect readability or safety.
