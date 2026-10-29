# AI Project Standards Starter

A reusable starter template for building new software projects with AI-assisted development aligned to a clear architecture and quality standard.

This repository includes a curated set of default GitHub Copilot guidance files, instruction documents, and specialized review agents designed to help teams bootstrap projects consistently across frontend, backend, database, testing, security, and performance concerns.

## Why this project exists

Starting a new project from scratch often leads to inconsistent structure, missing guardrails, and unclear standards. This template helps solve that by providing ready-to-use default rules for AI agents and human developers.

The repository is designed to help you:

- create new projects with a consistent architecture
- define clear frontend/backend boundaries
- enforce layered module design
- standardize testing, security, and performance expectations
- guide AI tools with explicit instructions and review checklists

## Included standards

The repository includes:

- `AGENTS.md` — project-level guidance and architecture overview
- `.github/copilot-instructions.md` — general Copilot instructions for the project
- `.github/instructions/` — domain rules for backend, frontend, database, security, performance, testing, and code review
- `.github/agents/` — specialized reviewer agents for architecture, backend, frontend, security, performance, and testing
- language-specific guidance under `.github/instructions/*` and `.github/agents/*` for Java, React, and Python projects

## Recommended architecture

This starter promotes a modular setup inspired by modern enterprise patterns:

```text
frontend/
  micro-uis/<domain>/
backend/
  services/<domain-service>/
shared-contracts/
infra/
```

Core principles:

- separate frontend and backend concerns
- organize by business domain
- keep layered boundaries within each module
- define explicit contracts and shared interfaces
- prefer secure, testable, and scalable defaults

## Structure

```text
.
├── AGENTS.md
├── LICENSE.md
├── LICENSE_APACHE_2.md
├── LICENSE_BSD_3C.md
├── LICENSE_GNU_LGPL_3.md
├── .github/
│   ├── copilot-instructions.md
│   ├── instructions/
│   └── agents/
└── README.md
```

## How to use this repository

You can use this project as a base template for new repositories:

1. Copy the repository or fork it as a starting point.
2. Adjust the architecture to fit your domain and product.
3. Customize the Copilot instructions and agent rules for your team.
4. Add your actual application code under `frontend/`, `backend/`, or service-specific folders.
5. Keep the existing instruction files as the default standard for AI-assisted work.

## Example workflow

When creating a new project with AI assistance:

- the AI agent reads `AGENTS.md` and `.github/copilot-instructions.md`
- it follows the standards in `.github/instructions/`
- it uses the recommended specialized agents for review and validation
- the project stays aligned with a known architecture and quality baseline

## Benefits

Using this repository as a standard starter gives your team:

- consistent project setup across teams and repositories
- better AI-generated code quality
- clearer architecture decisions from the beginning
- reusable governance for security, performance, and testing
- faster onboarding for new contributors

## License

This project includes several license files in the root directory. Please review them and choose the license that matches your intended usage before publishing or reusing this template.

## Contributing

You can extend this template with additional instruction files, project-specific rules, or domain-specific agents as your organization grows.

Feel free to contribute and add new programming languages, frameworks, or improvements to these instructions as you see fit. This is an open-source project, and the goal is to help everyone benefit from better, more productive AI-assisted development standards.

We encourage people to share improvements, optimizations, and new patterns so the project continues to evolve and support more teams and technologies.

## Summary

This repository is a practical AI-friendly starter for standardizing new software projects from day one. It gives teams a ready-made foundation for architecture, quality gates, and AI guidance without needing to rebuild those standards in every new project.
