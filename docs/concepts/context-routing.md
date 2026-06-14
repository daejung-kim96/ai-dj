# Context Routing

Context routing is the process of deciding which files an AI agent should read before it acts.

It is the first common skill because every other skill depends on it.

## The problem

AI agents can fail in two opposite ways:

- They read too little and miss important project rules.
- They read too much and lose focus.

Context routing solves this by choosing a small, relevant set of files first, then expanding only when needed.

## Basic flow

```text
User request
-> identify project profile
-> classify request type
-> choose required files
-> choose optional files
-> note missing context
-> proceed with selected skill
```

## Request types

Common request types:

- product/spec
- backend
- frontend
- data pipeline
- testing
- documentation
- git/release
- architecture review
- troubleshooting
- learning/concept explanation

## Required vs optional context

Required context is needed before acting.

Optional context may improve the result but should not block the first pass.

Missing context is information the agent expected but could not find.

```text
Required:
- project-profiles/my-app/project.yaml: identifies stack and source maps

Optional:
- docs/examples/form-page.md: useful if generating a similar page

Missing:
- API contract: needed to implement the frontend integration safely
```

## Expansion rule

Start narrow. Expand only when one of these is true:

- the selected files contradict each other
- a required source is missing
- implementation reaches an unknown boundary
- verification requires another file
- the user asks for a broader audit

## Why not read everything

Reading everything can make the agent slower and less precise. It can also mix unrelated rules from different projects.

The better pattern is:

```text
route -> read -> act -> verify -> expand if needed
```

## ai-dj usage

The `skills/context-router` skill should produce a routing plan before implementation or analysis begins.

The routing plan is not the final answer. It is a short context map used to make the next step grounded.
