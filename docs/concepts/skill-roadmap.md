# Skill Roadmap

This is the suggested order for building common ai-dj skills.

## Phase 1: Foundation

1. `context-router`: choose the right files before acting.
2. `generate-feature-context`: turn selected files into an implementation-ready context pack.
3. `verify-context-integrity`: check that output matches the selected context.
4. `update-living-documentation`: record durable learning and decisions.
5. `supervisor`: coordinate skill chains for broad requests.

## Phase 2: Engineering workflows

1. `testing-tdd`: convert behavior into tests first.
2. `backend-ddd`: model domains, aggregates, invariants, and repositories.
3. `backend-eda`: model events, producers, consumers, and idempotency.
4. `frontend-feature`: build pages, components, forms, states, and API integration.
5. `git-workflow`: prepare commits and PR summaries.

## Phase 3: Project profiles

After common skills are stable, add project profiles such as:

```text
project-profiles/artmoa/
  project.yaml
  required-reading-map.md
  domain-terms.md
  decisions.md
  known-issues.md
```

## Rule of thumb

Build skills in the order that reduces future confusion.

That is why `context-router` comes first: it decides what the agent should know before using any other skill.
