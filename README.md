# ai-dj

ai-dj is a reusable AI development harness repository.

It routes requests to project-aware skills, reads required context files, applies engineering workflows, verifies outputs, and records decisions as living documentation.

## Core idea

Use ai-dj as the control room for AI-assisted development across many projects.

- Common skills live in `skills/`.
- Common rules live in `docs/rules/`.
- Study notes and concept explanations live in `docs/concepts/` and are written in Korean by default.
- Project-specific context lives in `project-profiles/`.
- Verification rules live in `evals/`.
- Work summaries and audits live in `reports/`.

## Request flow

```text
User request
-> Read AGENT.md
-> Classify request type
-> Select relevant skill
-> Load required project profile context
-> Execute workflow
-> Verify output
-> Update living documentation
```

## First principles

- Read files before making claims.
- Prefer project rules over generic assumptions.
- Keep generated changes traceable to source context.
- Verify outputs before finalizing.
- Record durable decisions.
- Keep project-specific knowledge outside common skills.

## Starting structure

```text
ai-dj/
  AGENT.md
  README.md
  docs/
    concepts/
    rules/
    specs/
    examples/
    patch-notes/
  skills/
    supervisor/
    context-router/
    generate-feature-context/
    update-living-documentation/
    verify-context-integrity/
    backend-ddd/
    backend-eda/
    frontend-feature/
    frontend-verification/
    testing-tdd/
    git-workflow/
  project-profiles/
  evals/
  reports/
```
