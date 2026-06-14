# Skill: Update Living Documentation

Use this skill when work creates durable knowledge.

## Goal

Turn useful decisions, lessons, rules, and workflow changes into durable documentation.

This skill prevents important knowledge from staying only in the chat.

## When to use

Use this skill when work creates or changes:

- architecture decisions
- project decisions
- naming conventions
- business rules
- test policies
- known failures and fixes
- project setup requirements
- reusable workflow rules
- AI harness concepts
- skill behavior
- project profile facts

Do not use this skill for temporary notes or unverified guesses.

## Inputs

- User request
- Work summary
- Context integrity report, if available
- Files read
- Files changed
- Decisions made
- Open questions
- Active project profile, if any

## Durable knowledge categories

| Category | Examples | Default location |
| --- | --- | --- |
| concept | study explanation, mental model, glossary | `docs/concepts/` |
| common rule | cross-project rule, safety rule, routing rule | `docs/rules/` |
| skill workflow | reusable steps, templates, anti-patterns | `skills/<skill>/SKILL.md` |
| project decision | domain rule, stack choice, API policy | `project-profiles/<project>/decisions.md` |
| project vocabulary | domain terms, aliases, state meanings | `project-profiles/<project>/domain-terms.md` |
| known issue | repeated failure, workaround, risk | `project-profiles/<project>/known-issues.md` |
| patch note | change to ai-dj itself | `docs/patch-notes/` |
| report | one-off audit or generated review | `reports/` |

## Placement rules

- Put study-oriented explanations in `docs/concepts/`, in Korean by default.
- Put reusable cross-project rules in `docs/rules/`.
- Put reusable procedures in `skills/`.
- Put project-specific facts in `project-profiles/<project>/`.
- Put one-off outputs in `reports/`.
- Put ai-dj change history in `docs/patch-notes/`.
- Do not put project-specific facts into common skills.

## Steps

1. Identify what knowledge was created or changed.
2. Decide whether it is durable or temporary.
3. Classify the durable knowledge category.
4. Choose the target document.
5. Add or update a concise dated entry when useful.
6. Include source, reason, or verification basis.
7. Link the entry to the related skill, rule, project profile, or patch note.
8. Keep unresolved questions separate from confirmed decisions.
9. Avoid secrets, private data, and raw temporary logs.
10. Summarize what documentation was updated.

## Output format

```text
Living Documentation Update

Durable knowledge:
- item:
- category:
- source/reason:

Target documents:
- path: change made

Not recorded:
- item: why it should remain temporary

Open questions:
- question: target owner or next step

Verification:
- check: result
```

## Entry templates

### Decision entry

```text
## YYYY-MM-DD: Decision title

Decision:
- ...

Reason:
- ...

Source:
- ...

Impact:
- ...
```

### Known issue entry

```text
## YYYY-MM-DD: Issue title

Symptom:
- ...

Cause:
- ...

Fix or workaround:
- ...

Prevention:
- ...
```

### Concept entry

```text
# Concept Name

Short definition.

## Why it matters

...

## How ai-dj uses it

...
```

## Quality checklist

- The target location matches the knowledge category.
- The entry is short enough to find later.
- Confirmed decisions and open questions are separated.
- The entry includes a reason, source, or verification basis.
- No secrets or private data are recorded.
- Project-specific knowledge stays in the project profile.

## Anti-patterns

- Recording every temporary thought.
- Writing long narrative logs instead of searchable entries.
- Mixing confirmed decisions with speculation.
- Hiding unresolved questions.
- Putting project-specific rules into common skills.
- Copying command output into docs without a reason.

## Example

```text
Living Documentation Update

Durable knowledge:
- item: Study-oriented concept docs should be written in Korean by default.
- category: common documentation rule
- source/reason: user confirmed the docs are for learning and review.

Target documents:
- AGENT.md: added the Korean-by-default rule for concept docs
- README.md: documented the same convention
- docs/patch-notes/2026-06-14-korean-concepts.md: recorded the change

Not recorded:
- raw GitHub CLI auth logs: temporary and not useful for future work

Open questions:
- none

Verification:
- git status: clean after commit and push
```
