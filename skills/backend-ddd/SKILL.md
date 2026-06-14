# Skill: Backend DDD

Use this skill for backend work involving domain modeling, aggregates, entities, value objects, invariants, repositories, and application services.

## Goal

Turn feature behavior into a backend domain model with clear boundaries, explicit invariants, and testable state transitions.

This skill should keep business rules in the domain layer and infrastructure details at the edges.

## When to use

Use this skill when the user asks to:

- design or implement backend domain logic
- identify aggregates, entities, value objects, or repositories
- model state transitions, permissions, policies, or invariants
- separate domain behavior from application orchestration
- review whether backend code follows DDD boundaries
- convert a feature context or TDD plan into a domain design

Do not use this skill for simple CRUD work when there are no meaningful business rules, state transitions, or invariants.

## Inputs

- User request, issue, or feature context pack
- TDD plan from `skills/testing-tdd`, if behavior has been defined
- Domain specs, API contracts, existing domain code, and tests
- Active project profile, if any
- Existing naming, layering, persistence, and repository conventions

## Required context

At minimum, identify:

- bounded context or module
- domain language used by the project
- actor, command, or use case
- aggregate root candidate
- entities and value objects
- invariants that must always hold
- state transitions
- permissions and ownership rules
- repository boundary and persistence concerns
- domain tests or TDD cases that protect the model

If any required context is missing, list it as an open question instead of inventing it.

## Core distinctions

| Concept | Use it for |
| --- | --- |
| Bounded context | A business area where terms have one consistent meaning |
| Ubiquitous language | The shared language used in code, docs, and conversations |
| Entity | An object with identity across time |
| Value object | An immutable value defined by its attributes |
| Aggregate | A consistency boundary around related domain objects |
| Aggregate root | The object other code uses to access or change the aggregate |
| Invariant | A rule that must always be true after every valid change |
| Repository | A collection-like boundary for loading and saving aggregate roots |
| Application service | Orchestrates a use case, transaction, repository calls, and integrations |
| Domain service | Holds domain behavior that does not naturally belong to one entity or value object |

## Workflow

1. Restate the use case and bounded context in one sentence.
2. List source references and existing code/tests actually read.
3. Extract domain terms and align names with the project language.
4. Extract behavior, invariants, permissions, and state transitions from the feature context or TDD plan.
5. Choose the aggregate root by finding the consistency boundary, not by copying database tables.
6. Classify supporting concepts as entities, value objects, domain services, or policies.
7. Define commands/use cases at the application service boundary.
8. Define how the aggregate changes state and which methods enforce invariants.
9. Define repository boundaries around aggregate roots only.
10. Keep persistence, framework, API, queue, and external service details outside the domain model.
11. Feed newly discovered invariants back into `skills/testing-tdd` as domain tests.
12. Verify the model against context, tests, and project conventions.

## TDD connection

Use `skills/testing-tdd` before this skill when behavior is not already protected by tests.

Use this skill after the TDD plan to decide:

- which aggregate owns the behavior
- which invariant the failing test protects
- which state transition should happen
- which repository query or save boundary is needed
- which application service should orchestrate the use case

If DDD modeling discovers a new rule, return to `skills/testing-tdd` and add a failing domain test before implementing it.

## Output format

```text
Backend DDD Plan

Use case:
- summary:
- source:

Bounded context:
- name:
- reason:

Ubiquitous language:
- term: meaning/source

Aggregate:
- root:
- boundary:
- why this boundary:

Domain model:
- entities:
- value objects:
- domain services/policies:

Invariants:
- rule:
- enforced by:
- protected by test:

State transitions:
- from:
- command:
- to:
- invalid transitions:

Application boundary:
- command/use case:
- inputs:
- transaction boundary:
- orchestration responsibilities:

Repository boundary:
- repository:
- aggregate root loaded/saved:
- queries needed:

Tests:
- domain test:
- expected failing behavior:
- focused command:

Open questions:
- question:

Risks:
- risk: mitigation
```

## Quality checklist

- The model is based on source context, not generic DDD guesses.
- Domain terms match the project language.
- Aggregate boundaries protect real invariants.
- State transitions and invalid transitions are explicit.
- Application orchestration is separate from domain behavior.
- Repositories load and save aggregate roots, not arbitrary internal objects.
- Persistence and framework details do not leak into domain rules.
- Domain tests protect invariants and state transitions.
- Missing rules or ambiguous ownership are listed as open questions.
- Project-specific facts stay in the project profile or source references.

## Anti-patterns

- Treating every database table as an aggregate.
- Creating anemic domain models where services do all business behavior.
- Putting transaction, ORM, HTTP, or queue details inside domain objects.
- Making one large aggregate because objects are related.
- Inventing DDD terms that do not appear in the project language.
- Skipping tests for invariants because the model "looks right."
- Hiding unclear business rules inside permissive default behavior.

## Example

```text
Backend DDD Plan

Use case:
- summary: A user creates one review for a completed item.
- source: docs/specs/reviews.md

Bounded context:
- name: Reviews
- reason: Review terms and rules are separate from catalog browsing.

Aggregate:
- root: Review
- boundary: one review and its rating/content state
- why this boundary: duplicate review and ownership rules must stay consistent.

Invariants:
- rule: one user cannot create two reviews for the same item
- enforced by: review creation application service plus repository uniqueness check
- protected by test: rejects duplicate review by same user for same item

State transitions:
- from: none
- command: create review
- to: review created
- invalid transitions: duplicate review, unauthenticated user, item not completed

Application boundary:
- command/use case: CreateReview
- inputs: user id, item id, rating, content
- transaction boundary: duplicate check and save review
- orchestration responsibilities: load needed state, call domain creation, persist aggregate

Repository boundary:
- repository: ReviewRepository
- aggregate root loaded/saved: Review
- queries needed: existsByUserAndItem

Tests:
- domain test: duplicate review is rejected
- expected failing behavior: duplicate rule is not enforced yet
- focused command: run review domain tests

Open questions:
- Should duplicate attempts map to a conflict response at the API layer?
```
