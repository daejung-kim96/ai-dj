# Skills

A skill is a reusable workflow that tells an agent how to do a class of work.

It is not project-specific knowledge. It is the repeatable method.

## Example

`backend-ddd` should explain how to reason about aggregates, invariants, commands, repositories, and tests.

It should not say that a specific project has an `Exhibition` aggregate. That belongs in a project profile.

## Skill anatomy

A useful skill usually contains:

- when to use it
- goal
- inputs
- required context
- step-by-step workflow
- output format
- verification checklist
- examples or anti-patterns

## Common vs project-specific

```text
Common skill:
  "For DDD work, identify bounded context and aggregate invariants."

Project profile:
  "In this project, exhibition approval uses PENDING, APPROVED, REJECTED."
```

Keeping this separation lets ai-dj work across many projects.

## Skill chaining

Skills can be used together.

Example:

```text
supervisor
-> context-router
-> generate-feature-context
-> backend-ddd
-> testing-tdd
-> verify-context-integrity
-> update-living-documentation
```

The supervisor chooses the chain. Each skill should stay focused.

## Study note

When adding a new skill, ask:

1. Is this workflow useful in more than one project?
2. What files must the agent read before using it?
3. What output should it produce?
4. How can we tell if the output is good?
5. What should be recorded for future work?
