# Skill: Context Router

Use this skill to decide which files an agent must read before acting.

## Goal

Produce a small, relevant context plan for the next step.

The context router should prevent two common failures:

- acting without enough project context
- reading too much and mixing unrelated rules

## Inputs

- User request
- Active project profile
- Available source map
- Selected skill

## Request classification

Classify the request into one primary type and optional secondary types.

| Type | Use when the user asks about | Common context |
| --- | --- | --- |
| product/spec | requirements, user stories, acceptance criteria | specs, product docs, decisions |
| backend | domain, API, persistence, auth, events | backend rules, API contracts, domain docs |
| frontend | pages, components, forms, state, UI behavior | frontend rules, design docs, API contracts |
| data-pipeline | crawling, ETL, CSV, cleaning, validation | scripts, data README, schema notes |
| testing | unit, integration, E2E, TDD, regression | test rules, specs, target code |
| documentation | README, decisions, docs cleanup | docs rules, existing docs |
| git-release | commits, PRs, changelog, versioning | git workflow, changed files |
| architecture-review | structure, boundaries, tradeoffs | rules, diagrams, decisions, source map |
| troubleshooting | error, failing test, broken workflow | logs, target files, recent changes |
| learning | concept explanation, study notes | concepts, rules, examples |

## Steps

1. Identify keywords, domain objects, and task type.
2. Identify the active project profile, if any.
3. Read the project `required-reading-map.md` if a profile exists.
4. Select only the files needed for the first pass.
5. Mark optional files separately.
6. Mark missing files or unavailable sources separately.
7. Explain why each required file matters.
8. Expand context only when blocked or when verification requires it.

## Selection rules

- Prefer project profile maps over guessing.
- Prefer source-of-truth files over generated summaries.
- Prefer the smallest useful file set for the first pass.
- Do not load another project's profile unless the user asks for cross-project comparison.
- Use optional context when it may help but should not block progress.
- Treat missing required context as a risk, not as permission to invent details.

## Expansion triggers

Expand the context only when:

- required context is missing
- selected files disagree
- a boundary between domains is unclear
- a tool or test result points to another file
- the user asks for a broad review
- the output needs verification from another source

## Output format

```text
Request type:
- primary:
- secondary:

Required:
- path: reason

Optional:
- path: reason

Missing:
- expected path or source: why it matters

Next skill:
- skill path: reason
```

## Example

```text
Request:
"Build the review write flow."

Request type:
- primary: backend
- secondary: testing, product/spec

Required:
- project-profiles/my-app/project.yaml: identifies stack and source roots
- project-profiles/my-app/required-reading-map.md: maps feature requests to source docs
- docs/specs/reviews.md: source behavior and acceptance criteria
- api/openapi.yaml: API contract for review creation

Optional:
- docs/examples/domain-test.md: useful if adding domain tests

Missing:
- UI flow spec: needed only if the task expands to frontend work

Next skill:
- skills/generate-feature-context: prepare implementation context before coding
```
