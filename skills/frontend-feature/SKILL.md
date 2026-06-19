# Skill: Frontend Feature

Use this skill for frontend pages, components, forms, client/server state, user flows, accessibility, responsive behavior, and API integration.

## Goal

Turn feature behavior into a usable frontend implementation plan with clear user flow, UI states, component boundaries, API contracts, and verification steps.

This skill should prevent frontend work from becoming a static screen that only handles the happy path.

## When to use

Use this skill when the user asks to:

- design or implement a frontend page, screen, modal, form, or component
- connect UI to backend APIs or event-driven state changes
- model loading, empty, error, success, disabled, and permission states
- review frontend architecture or component boundaries
- define form validation and submission behavior
- check responsive behavior, accessibility, and visual fit
- convert feature context or TDD scenarios into UI behavior

Do not use this skill only to make cosmetic changes unless the change affects user workflow, state, accessibility, or design consistency.

## Inputs

- User request, issue, or feature context pack
- TDD plan from `skills/testing-tdd`, if behavior has been defined
- API contracts, backend rules, error codes, and permission rules
- Existing routes, pages, components, hooks, stores, and design system conventions
- Active project profile, if any
- UI screenshots, Figma/design references, or existing similar screens when available

## Required context

At minimum, identify:

- route, screen, modal, or component scope
- primary user goal and user flow
- entry points and exit points
- API dependencies and contract certainty
- server state and client UI state
- loading, empty, error, success, disabled, and unauthorized states
- form fields, validation, submit behavior, and reset behavior
- component boundaries and reuse opportunities
- accessibility requirements
- responsive layout risks
- tests, browser checks, or visual verification steps

If any required context is missing, list it as an open question instead of inventing it.

## Core distinctions

| Concept | Use it for |
| --- | --- |
| User flow | The steps a user takes to complete the goal |
| Route/screen scope | The URL, page, modal, or UI region being changed |
| Server state | Data loaded from or saved to backend systems |
| Client UI state | Local interaction state such as open/closed, selected tab, draft input, focus |
| UI state | Loading, empty, error, success, disabled, optimistic, or unauthorized views |
| Component boundary | The responsibility and public props/events of a component |
| API contract | Request, response, error, validation, and permission shape agreed with backend |
| Form model | Fields, defaults, validation, dirty state, submit, success, and reset behavior |
| Accessibility | Keyboard, focus, label, semantics, contrast, and screen reader behavior |
| Responsive behavior | How layout adapts across mobile, tablet, desktop, and dense content |

## Workflow

1. Restate the user goal and frontend scope in one sentence.
2. List source references and existing UI/API files actually read.
3. Map the user flow from entry to completion and cancellation.
4. Separate server state from client UI state.
5. Enumerate UI states: loading, empty, error, success, disabled, unauthorized, and optimistic states when relevant.
6. Define API dependencies, request/response shape, validation errors, and permission errors.
7. If API or backend behavior is missing, stop and route the gap to backend context instead of inventing it.
8. Define form fields, validation, submit behavior, pending state, success handling, and error recovery.
9. Choose component boundaries based on responsibility, reuse, and existing project conventions.
10. Align layout, typography, spacing, icons, and controls with the existing design system.
11. Check accessibility: labels, keyboard navigation, focus management, semantics, contrast, and error announcements.
12. Check responsive behavior and text fit across expected viewport sizes.
13. Define tests and manual/browser verification steps.
14. Update durable documentation if a reusable UI rule or backend contract decision is discovered.

## Backend feedback loop

Frontend work often exposes missing backend context.

Do not guess when these are unclear:

- endpoint path or method
- request body or query parameters
- response shape
- pagination or sorting contract
- validation error format
- permission or ownership rule
- status transition rule
- event timing or eventual consistency behavior

When backend context is missing, record the gap and route to the right skill:

```text
missing API contract -> generate-feature-context or backend-ddd
missing domain rule -> backend-ddd
missing async/event timing -> backend-eda
missing test behavior -> testing-tdd
```

## Output format

```text
Frontend Feature Plan

Scope:
- route/screen/component:
- user goal:
- entry/exit:

Source references:
- path: purpose

User flow:
- step:
- expected UI:

API dependencies:
- endpoint:
- request:
- response:
- errors:
- open contract questions:

State model:
- server state:
- client UI state:
- derived state:

UI states:
- loading:
- empty:
- error:
- success:
- disabled:
- unauthorized:

Form behavior:
- fields:
- validation:
- submit:
- pending:
- success handling:
- error recovery:

Component plan:
- component:
- responsibility:
- props/events:
- reuse:

Accessibility:
- keyboard:
- focus:
- labels/semantics:
- error announcement:

Responsive/visual checks:
- mobile:
- desktop:
- text fit:
- design system alignment:

Verification:
- tests:
- browser/manual checks:

Backend follow-ups:
- question:

Risks:
- risk: mitigation
```

## Quality checklist

- The plan starts from user flow, not component names.
- API contracts are cited or marked as open questions.
- Server state and client UI state are separated.
- Non-happy states are defined, especially loading, empty, error, unauthorized, and disabled states.
- Form validation and submit lifecycle are explicit.
- Component boundaries match existing project conventions.
- Accessibility and focus behavior are considered.
- Responsive layout and text fit are verified or planned.
- Backend assumptions are not invented.
- Verification includes tests, browser checks, or explicit manual checks.
- Project-specific UI rules stay in project profiles or source references.

## Anti-patterns

- Building only the success screen.
- Inventing API fields because the UI needs them.
- Mixing fetched server data with local UI state without clear ownership.
- Hiding form errors in generic messages.
- Creating one large page component that owns every concern.
- Ignoring keyboard navigation and focus after modals, errors, or route changes.
- Treating responsive behavior as "CSS will handle it."
- Changing visual style without checking existing components or design rules.

## Example

```text
Frontend Feature Plan

Scope:
- route/screen/component: exhibition detail review form
- user goal: write one review for an exhibition
- entry/exit: open from exhibition detail, return to review list after success

User flow:
- user opens review form
- form loads existing eligibility state
- user enters rating and content
- user submits
- UI shows pending state
- success closes form and shows new review
- duplicate review error shows actionable message

API dependencies:
- endpoint: POST /reviews
- request: exhibitionId, rating, content
- response: review id, rating, content, author, createdAt
- errors: duplicate review, invalid rating, unauthorized
- open contract questions: exact duplicate review error code

State model:
- server state: exhibition, current user, existing review eligibility, created review
- client UI state: rating draft, content draft, dirty state, submit pending, inline errors
- derived state: submit disabled when invalid or pending

UI states:
- loading: eligibility check pending
- empty: no existing reviews yet
- error: failed to load eligibility or failed submit
- success: review appears in list and form resets
- disabled: submit disabled while invalid or pending
- unauthorized: prompt login instead of showing writable form

Backend follow-ups:
- confirm duplicate review error code and response shape before implementing final error mapping
```