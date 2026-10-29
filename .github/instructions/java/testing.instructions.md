---
applyTo: "**/*Test.java,**/*Tests.java,**/src/test/**/*.java"
description: "Java-specific testing add-on for the generic testing instructions. This file complements .github/instructions/testing.instructions.md and should only include Java-specific rules."
---

# Java Testing Add-on

> This file complements `.github/instructions/testing.instructions.md`.
> It is not a replacement for the generic testing instructions and should only add Java-specific guidance.

## Java testing strategy

- Add unit tests for validation logic and domain rules.
- Add integration tests for service and controller flows that depend on persistence or external boundaries.
- Cover failure paths as well as success paths.
- Keep tests deterministic and isolated.

## Java test structure and style

- Use clear Arrange/Act/Assert sections.
- Prefer readable test names that describe the scenario.
- Reuse fixtures and setup helpers instead of duplicating test data.
- Test behavior, not internal implementation details.

## Java regression safety

- Add tests for bug fixes and risky code changes.
- Prefer focused tests over broad, brittle test suites.
- Keep tests understandable enough to serve as executable specification.
