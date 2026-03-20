# Kiro Spec-Driven Development Artifacts

This directory contains the spec files used to build the interactive prep exam for the AWS Certified Generative AI Developer – Professional certification. They were created and managed using [Kiro](https://kiro.dev), an AI-powered IDE that uses a structured, spec-driven development workflow.

These files are included here to demonstrate the methodology — showing how Kiro and Claude models were used together to go from a blank project to a fully functional educational tool.

## Files

- **requirements.md** — Defines *what* to build. Contains user stories and acceptance criteria that describe the desired features and behavior of the prep exam application.
- **design.md** — Defines *how* to build it. Describes the technical architecture, component design, data models, and correctness properties that guide the implementation.
- **tasks.md** — The incremental implementation plan. A checklist of tasks (with sub-tasks) that break the design into small, ordered steps for iterative development.

## About Kiro's Spec-Driven Development

Kiro's spec-driven workflow follows a structured progression:

1. **Requirements** — You describe what you want to build. Kiro helps you define user stories and acceptance criteria in `requirements.md`.
2. **Design** — Kiro generates a technical design in `design.md`, covering architecture, components, data models, and testable correctness properties based on the requirements.
3. **Tasks** — Kiro breaks the design into an ordered implementation plan in `tasks.md`, with each task scoped to be small and independently verifiable.
4. **Implementation** — You work through the tasks one at a time. Kiro assists with code generation, testing, and validation at each step.

This iterative approach keeps AI-assisted development grounded in well-defined specifications, making the process repeatable and the output predictable.
