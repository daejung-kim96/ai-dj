# Skill: Generate Feature Context

Use this skill before implementing, testing, reviewing, or planning a feature.

## Goal

Create an implementation-ready context pack for one feature or request.

This skill receives selected context from `skills/context-router` and turns it into a compact working brief.

## When to use

Use this skill when the user asks to:

- implement a feature
- plan a feature
- review a feature design
- create tests from requirements
- compare requirements with API or code
- prepare backend or frontend work

Do not use this skill for broad strategy work unless the request can be narrowed to one feature or workflow.

## Inputs

- Feature ID, user story, issue, or natural language request
- Context-router output
- Active project profile, if any
- Relevant specs, API docs, architecture rules, tests, and source files

## Required context

At minimum, try to identify:

- feature identity or name
- source references
- user goal or business goal
- affected domain objects
- affected API or command surface
- data/state rules
- permission or role rules
- UI or interaction impact
- test expectations
- open questions

If any required context is missing, list it explicitly instead of inventing details.

## Steps

1. Restate the feature in one sentence.
2. List source files that were actually read.
3. Extract user goal, acceptance criteria, and workflow.
4. Identify domain objects, state values, permissions, and invariants.
5. Identify API surface, commands, events, or integration points.
6. Identify UI implications if frontend behavior is affected.
7. Derive test scenarios from behavior and risk.
8. Separate confirmed facts from assumptions.
9. List open questions and blockers.
10. Recommend the next skill or workflow.

## Output format

```text
Feature Context

Feature:
- id/name:
- summary:

Source references:
- path: what was used

Confirmed facts:
- fact: source

User/workflow:
- actor:
- trigger:
- expected outcome:

Domain/data:
- objects:
- state values:
- invariants:
- permissions:

API/integration:
- endpoint/command/event:
- request:
- response:
- errors:

UI impact:
- route/screen:
- states:
- interactions:

Tests:
- scenario:
- expected result:

Assumptions:
- assumption: why it is not confirmed

Open questions:
- question: why it matters

Risks:
- risk: mitigation

Next skill:
- skill path: reason
```

## Quality checklist

- The context pack is based on files that were actually read.
- Source references are specific enough to trace.
- Facts and assumptions are separated.
- Missing information is visible.
- The output is short enough to use as a working brief.
- The next skill is explicit.
- Project-specific facts stay in the project profile or source references.

## Anti-patterns

- Turning the context pack into a long document summary.
- Treating guesses as confirmed facts.
- Skipping permissions, state values, or error cases.
- Mixing unrelated project rules into the context.
- Preparing implementation steps before the feature identity is clear.

## Example

```text
Feature Context

Feature:
- id/name: REV-01 review creation
- summary: A logged-in user writes a rating and text review for a completed item.

Source references:
- docs/specs/reviews.md: acceptance criteria
- api/openapi.yaml: POST /reviews contract
- project-profiles/my-app/decisions.md: review ownership rule

Confirmed facts:
- Only logged-in users can create reviews: api/openapi.yaml
- A review has rating and content: docs/specs/reviews.md

Assumptions:
- The UI submits from the detail screen: route spec not found

Open questions:
- Can one user write multiple reviews for the same item? This affects domain invariants and tests.

Next skill:
- skills/testing-tdd: convert the confirmed behavior into tests first
```
