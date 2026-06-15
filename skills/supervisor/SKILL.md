# Skill: Supervisor

Use this skill when the request is broad, unclear, or spans multiple skills.

## Goal

Turn a user request into a concrete workflow with the right context, skill sequence, verification, and documentation updates.

The supervisor coordinates work. It should not replace the specialized skills.

## When to use

Use this skill when the user asks to:

- plan work
- implement a feature
- review architecture
- compare options
- update the harness
- decide what to do next
- perform work that spans multiple skill areas
- handle an unclear request that needs routing

Do not use this skill when a single narrow skill is obviously sufficient and the workflow is already clear.

## Inputs

- User request
- Active repository or project
- Available project profiles
- Current skill inventory
- Relevant rules, concepts, and evals
- Prior context from the current task

## Core operating loop

```text
clarify objective
-> identify project profile
-> classify request
-> choose skill chain
-> run context-router
-> run task-specific skills
-> verify context integrity
-> update living documentation if needed
-> summarize outcome and next step
```

## Request types and default chains

| Request type | Default chain |
| --- | --- |
| broad planning | `context-router -> generate-feature-context -> verify-context-integrity` |
| feature implementation | `context-router -> generate-feature-context -> testing-tdd -> engineering skill -> verify-context-integrity -> update-living-documentation` |
| backend domain work | `context-router -> generate-feature-context -> testing-tdd -> backend-ddd -> verify-context-integrity` |
| event-driven backend work | `context-router -> generate-feature-context -> testing-tdd -> backend-ddd -> backend-eda -> verify-context-integrity` |
| frontend work | `context-router -> generate-feature-context -> testing-tdd -> frontend-feature -> verify-context-integrity` |
| concept learning | `context-router -> update docs/concepts -> verify-context-integrity -> update-living-documentation` |
| documentation update | `context-router -> update-living-documentation -> verify-context-integrity` |
| git/release work | `context-router -> git-workflow -> verify-context-integrity` |
| project profile setup | `context-router -> generate-feature-context if needed -> update-living-documentation -> verify-context-integrity` |

If a listed engineering skill is still immature, produce the plan and mark the missing skill capability explicitly.

## Skill selection rules

- Choose the smallest skill chain that can satisfy the request.
- Prefer foundation skills before engineering skills.
- Use the active project profile when one exists.
- Keep project-specific facts out of common skills.
- Add `update-living-documentation` only when durable knowledge is created or changed.
- Add `verify-context-integrity` before finalizing project-specific or durable work.
- Stop and ask the user only when missing context makes a reasonable assumption risky.

## Stop conditions

Stop and surface the issue when:

- the active project profile is required but missing
- required source files cannot be found
- source files contradict each other on a P0 behavior
- the request asks for unsafe production-side effects
- the chosen skill does not exist and no safe fallback is available
- the user needs to make a business or product decision

## Steps

1. Restate the objective in one sentence.
2. Identify whether the task is generic or tied to a project profile.
3. Classify the request type.
4. Select the smallest useful skill chain.
5. Run or request `context-router` to identify required files.
6. Run the selected task-specific skill or prepare a plan if the skill is not ready.
7. Run `verify-context-integrity` for project-specific or durable outputs.
8. Run `update-living-documentation` if the task created durable knowledge.
9. Summarize work completed, verification, documents updated, and next action.

## Output format

```text
Supervisor Plan

Objective:
- ...

Project profile:
- active profile or generic

Request type:
- primary:
- secondary:

Skill chain:
- skill: reason

Required context:
- path or source: reason

Execution notes:
- ...

Verification:
- status:
- findings:

Documentation updates:
- path: change

Next action:
- ...
```

## Quality checklist

- The objective is concrete.
- The selected chain is not larger than needed.
- Context routing happens before project-specific claims.
- Missing context is visible.
- Verification is included when output depends on source truth.
- Durable decisions or learning are documented.
- The final answer tells the user what changed and what to do next.

## Anti-patterns

- Running every skill for every request.
- Skipping context routing because the request sounds simple.
- Using supervisor as a replacement for specialized skills.
- Letting a plan become the final output when implementation was requested.
- Recording temporary notes as durable documentation.
- Asking the user to decide things that can be discovered from files.

## Example

```text
Supervisor Plan

Objective:
- Prepare a workflow for adding review creation to a project.

Project profile:
- project-profiles/my-app

Request type:
- primary: feature implementation
- secondary: backend, testing

Skill chain:
- context-router: find specs, API contracts, and project decisions
- generate-feature-context: create a working brief
- testing-tdd: turn behavior into tests
- backend-ddd: model invariants and domain behavior
- verify-context-integrity: check claims against sources
- update-living-documentation: record durable decisions if any are made

Required context:
- project-profiles/my-app/required-reading-map.md: maps feature requests to sources
- docs/specs/reviews.md: behavior and acceptance criteria
- api/openapi.yaml: API contract

Verification:
- status: pending until source files are read

Next action:
- run context-router for the review feature
```
