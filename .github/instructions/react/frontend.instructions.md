---
applyTo: "**/*.tsx,**/*.ts,**/*.jsx,**/*.js"
description: "React-specific frontend add-on for the generic frontend instructions. This file complements .github/instructions/frontend.instructions.md and only adds React-specific guidance not already covered there."
---

# React Frontend Add-on

> This file complements `.github/instructions/frontend.instructions.md`.
> It only adds React-specific guidance that is not already defined in the parent instructions.

## React component patterns

- Prefer function components and hooks over class components in new code.
- Keep components focused on rendering; move side effects and async logic into hooks or service modules.
- Keep props explicit and typed; avoid passing unstructured objects when a stable contract exists.
- Avoid inline data mutations inside render paths; compute values with pure functions when possible.

## State and data flow

- Prefer local state for UI-only concerns and lift state only when multiple components genuinely share it.
- Centralize asynchronous data fetching in hooks or adapter layers instead of scattering fetch logic across components.
- Handle loading, error, and empty states consistently for each async flow.
- Use stable keys and predictable state transitions for lists, forms, and filtered views.

## React rendering and effects

- Keep `useEffect` focused on synchronization, not business logic or derived state updates.
- Avoid unnecessary re-renders by memoizing expensive derived values only when profiling shows benefit.
- Do not trigger network calls in render paths or from effect chains that re-run on every render without guard conditions.
- Prefer controlled inputs and explicit state transitions for forms and interactive widgets.

## Error and accessibility boundaries

- Use accessible patterns for interactive controls, focus handling, and error messaging.
- Ensure form and validation errors are announced clearly to assistive technologies.
- Fail gracefully when async data or user input is missing; avoid silent fallback behavior in user-facing flows.

## React-specific architecture guidance

- Organize feature code by domain or route, with components, hooks, and service adapters kept close to each other.
- Keep reusable UI logic in a shared layer, but avoid over-generalizing before the abstraction is needed.
- Use a clear boundary between view components and side-effect-heavy logic.
- Prefer small, composable components over deeply nested conditional render trees.
