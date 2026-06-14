# Skill: Verify Context Integrity

Use this skill to check whether generated work matches the selected context.

## Goal

Detect mismatches between the user's request, selected context, generated output, and durable documentation.

This skill should make uncertainty visible before work is considered done.

## When to use

Use this skill after:

- context routing
- feature context generation
- code generation or edits
- documentation updates
- skill updates
- project profile changes
- architecture or API recommendations

Use it before finalizing work that depends on project-specific facts.

## Inputs

- User request
- Context-router output
- Feature context pack, if one was generated
- Files actually read
- Files changed
- Active project profile, if any
- Relevant source docs, rules, tests, or contracts

## Integrity dimensions

Check the work across these dimensions.

| Dimension | Question |
| --- | --- |
| source coverage | Were required files actually read? |
| traceability | Can each important claim point to a source? |
| consistency | Do names, permissions, states, APIs, and terms match? |
| assumption control | Are assumptions separated from confirmed facts? |
| missing context | Are gaps visible instead of hidden? |
| project boundary | Did project-specific facts stay out of common skills? |
| documentation | Were durable decisions recorded? |
| verification | Were relevant tests, checks, or manual reviews run or described? |
| safety | Did the work avoid secrets and production side effects? |

## Severity

Use three severity levels.

```text
P0 fail:
  The output is not trustworthy until fixed.

P1 warning:
  The output can be used, but there is a meaningful gap or open question.

P2 note:
  Minor improvement, polish, or follow-up.
```

## Status

Return one overall status.

```text
pass:
  No blocking findings. Remaining notes are minor.

warning:
  No blocking mismatch, but one or more P1 findings remain.

fail:
  At least one P0 finding exists, or required context was missing.
```

## Steps

1. Restate what is being verified.
2. List the files and context artifacts used for verification.
3. Check that required context was read.
4. Check source traceability for important claims.
5. Check consistency of names, permissions, state values, API shape, and domain terms.
6. Check that confirmed facts, assumptions, open questions, and risks are separated.
7. Check that project-specific facts did not move into common skills.
8. Check that durable decisions or concept changes were recorded.
9. Check that verification steps were run or explicitly described.
10. Produce findings with severity, source, issue, and suggested fix.

## Output format

```text
Context Integrity Report

Status: pass | warning | fail

Verified target:
- request:
- artifacts:

Sources checked:
- path: purpose

Findings:
- severity: source -> issue -> suggested fix

Missing context:
- source: why it matters

Assumptions still present:
- assumption: owner or next step

Verification performed:
- check: result

Documentation updates:
- path: what was recorded

Next action:
- recommended next step
```

## Pass criteria

A result can pass when:

- required context was read
- important claims are traceable
- project-specific facts stayed in source docs or project profiles
- assumptions and open questions are visible
- no P0 findings remain
- durable learning or decisions were recorded when needed
- verification was run or explicitly described

## Anti-patterns

- Marking work as pass because it "looks right."
- Reporting only problems without suggested fixes.
- Treating missing context as harmless.
- Hiding assumptions inside confident language.
- Verifying only changed files while ignoring the source references.
- Moving project-specific rules into common skills for convenience.

## Example

```text
Context Integrity Report

Status: warning

Verified target:
- request: prepare feature context for review creation
- artifacts: generated feature context pack

Sources checked:
- docs/specs/reviews.md: acceptance criteria
- api/openapi.yaml: review API contract
- project-profiles/my-app/decisions.md: ownership policy

Findings:
- P1 warning: docs/specs/reviews.md -> duplicate review policy is not defined -> ask whether one user can review the same item more than once
- P2 note: api/openapi.yaml -> 403 error is not listed -> confirm whether authorization failure should be 401 or 403

Missing context:
- UI route spec: needed only before frontend implementation

Assumptions still present:
- review form is on the detail screen: confirm with UI spec

Verification performed:
- source traceability check: passed
- state and permission consistency check: warning

Documentation updates:
- none: no durable decision was made

Next action:
- resolve duplicate review policy before backend-ddd work
```
