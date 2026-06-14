# Skill: Testing TDD

Use this skill when turning requirements, specs, bug reports, or feature context into tests before implementation.

## Goal

Convert expected behavior into a small, traceable test plan that can drive implementation.

This skill should make the intended behavior visible before code is changed.

## When to use

Use this skill when the user asks to:

- implement behavior test-first
- convert requirements into tests
- add coverage for a bug or regression
- prepare tests from a feature context pack
- check state transitions, permissions, validation, or error handling
- decide which test level is appropriate before coding

Do not use this skill only to increase coverage numbers without a behavior or risk to protect.

## Inputs

- User request, issue, bug report, or feature context pack
- Relevant specs, API contracts, domain rules, UI states, or source files
- Existing test conventions and test helpers
- Active project profile, if any
- Known risks, invariants, permissions, and edge cases

## Required context

At minimum, identify:

- behavior being protected
- source reference for the behavior
- actor or caller
- trigger or command
- expected success result
- expected failure result
- state changes or emitted events
- permissions and ownership rules
- existing test framework and naming pattern

If any required context is missing, write it as an open question instead of inventing it.

## Test level selection

Pick the smallest useful test level that can prove the behavior.

| Test level | Use when |
| --- | --- |
| Unit test | Pure functions, formatting, validation, or isolated logic |
| Domain test | Aggregate rules, invariants, state transitions, policies, or permissions |
| API contract test | Request/response shape, status codes, validation errors, or backward compatibility |
| Integration test | Database, queue, external adapter boundary, or multiple modules must work together |
| E2E test | A user workflow across UI, API, persistence, and side effects must be protected |

Prefer a lower-level test when it gives the same confidence with less cost.

## TDD loop

1. Restate the behavior in one sentence.
2. List the source references used.
3. Extract success paths, failure paths, edge cases, state transitions, and permissions.
4. Choose the smallest useful test level for each behavior.
5. Write the expected failing tests first.
6. Confirm why each test should fail before implementation.
7. Implement only the smallest change needed to pass.
8. Run the focused test command.
9. Refactor only after tests pass.
10. Update documentation if the behavior or decision should be remembered.

## Test case shape

Write test cases in behavior language.

```text
- given:
- actor/context:
- existing state:
- input:
- expected result:
- expected state:
- expected error/event:
- source:
```

## Output format

```text
TDD Plan

Behavior:
- summary:
- source:

Test cases:
- name:
  level:
  given:
  when:
  then:
  should fail because:

Implementation boundary:
- smallest change:
- files likely affected:

Verification:
- focused command:
- expected result:

Open questions:
- question:
```

## Quality checklist

- Tests are derived from source context, not guesses.
- At least one failing test is expected before implementation.
- Test names describe behavior, not implementation details.
- Success, failure, permissions, and state changes are considered when relevant.
- The chosen test level is the smallest useful level.
- Existing project test conventions are followed.
- Refactoring happens only after focused tests pass.
- Missing behavior is listed as an open question.

## Anti-patterns

- Writing implementation first and adding tests afterward while calling it TDD.
- Testing private implementation details instead of externally visible behavior.
- Using E2E tests for behavior that a domain or API test can prove.
- Creating brittle tests that duplicate the code structure.
- Treating generated tests as trustworthy without running or reviewing them.
- Ignoring existing test helpers, factories, fixtures, or naming conventions.

## Example

```text
TDD Plan

Behavior:
- summary: A reviewer cannot submit a second review for the same completed item.
- source: docs/specs/reviews.md

Test cases:
- name: rejects duplicate review by same user for same item
  level: Domain test
  given: user already has one review for item A
  when: user submits another review for item A
  then: duplicate review error is returned and no new review is stored
  should fail because: duplicate policy is not implemented yet

Implementation boundary:
- smallest change: add duplicate review invariant in review creation domain service
- files likely affected: review domain service and repository query

Verification:
- focused command: run the review domain test file
- expected result: new duplicate review test passes with existing tests

Open questions:
- Should duplicate attempts return 400, 409, or a domain-specific error code at the API layer?
```
