---
applyTo: "**/*"
description: "Generic frontend conventions for micro-UIs and presentation modules"
---

# Frontend Conventions

## Micro-UI Structure

- Organize frontend by **domain micro-UI**.
- Each micro-UI should isolate routes, components, and interface adapters.
- Keep a shell/composition layer for navigation, auth context, and shared layout.

Suggested shape:

```text
frontend/micro-uis/<domain>/
  components/
  pages/
  hooks/
  services/      # interface clients/adapters
  models/        # frontend domain/view models
  routes/
  tests/
```

## Component and State Rules

- Keep components small and focused.
- Co-locate component tests with components when practical.
- Prefer local state first; promote to shared/global state only when needed.
- Keep side effects in hooks/services, not inside presentational components.

## Data and Interface Rules

- Centralize external calls in service/adaptor modules.
- Use typed contracts for request/response payloads.
- Handle loading, empty, and error states explicitly in UI.

## UX and Accessibility

- Build keyboard-accessible interactions.
- Ensure semantic structure and accessible labels.
- Keep user-visible text ready for localization.

## Performance Basics

- Lazy-load micro-UIs and large routes.
- Avoid unnecessary global re-renders.
- Use memoization only when profiling shows value.