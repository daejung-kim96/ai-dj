---
name: frontend-verification
description: Verify frontend pages, components, forms, API integrations, visual changes, responsive behavior, accessibility, runtime health, and browser user flows after frontend implementation or UI changes. Use when Codex needs to prove the real browser experience, inspect loading/error/success/disabled/unauthorized states, check console/network/hydration/asset issues, or produce a frontend verification report.
---

# Skill: Frontend Verification

Use this skill after frontend implementation, UI changes, form changes, API integration, visual updates, or responsive/accessibility fixes to verify the real user experience in a browser or equivalent test surface.

## Goal

Prove that the frontend works beyond the happy path by checking behavior, visual fit, accessibility, responsive layout, console/runtime health, and API integration.

This skill should catch issues that static code review and unit tests often miss.

## When to use

Use this skill after:

- frontend feature implementation
- component or page changes
- form behavior changes
- API integration changes
- loading, empty, error, success, or permission state changes
- responsive layout changes
- accessibility or focus behavior changes
- design system or visual polish changes

Do not use this skill as a replacement for automated tests. Use it to verify real browser behavior and visible UX risks.

## Inputs

- User request or feature context pack
- Frontend plan from `skills/frontend-feature`, if available
- TDD scenarios from `skills/testing-tdd`, if behavior has been defined
- Changed files and affected routes/components
- Available dev server command and local URL
- API contracts, mock fixtures, seed data, or test accounts
- Design screenshots, Figma references, or existing similar screens when available

## Required context

At minimum, identify:

- target route, screen, modal, or component
- user goal being verified
- changed files or affected UI areas
- state scenarios to inspect
- viewport sizes to check
- browser/dev server command or reason browser verification cannot be run
- API/backend dependencies and mock data assumptions
- expected accessibility and focus behavior
- verification evidence to report

If required context is missing, list it as a verification gap instead of pretending the check passed.

## Verification dimensions

| Dimension | Check |
| --- | --- |
| Behavior | Does the user flow complete as expected? |
| UI states | Are loading, empty, error, success, disabled, and unauthorized states handled? |
| API integration | Are request, response, error, and pending behaviors correct? |
| Visual fit | Do text, buttons, cards, forms, and layout fit without overlap or clipping? |
| Responsive | Does the UI work across mobile, tablet, and desktop widths? |
| Accessibility | Are labels, keyboard navigation, focus, semantics, contrast, and error announcements acceptable? |
| Runtime health | Are there console errors, hydration errors, network failures, or broken assets? |
| Regression risk | Did nearby existing workflows still behave correctly? |

## Workflow

1. Restate the target UI and user flow being verified.
2. List changed files and source references used for expected behavior.
3. Identify the dev server command and local URL.
4. Start or reuse the dev server when needed.
5. Open the target route in a browser or equivalent verification surface.
6. Verify the happy path.
7. Verify non-happy states: loading, empty, error, unauthorized, disabled, validation, and retry states when relevant.
8. Verify API behavior: pending, success, validation error, auth error, server error, and stale/refetch behavior when relevant.
9. Check console, network, hydration, and asset errors.
10. Check responsive viewports, at least mobile and desktop when possible.
11. Check text fit, layout overlap, scroll behavior, and long-content behavior.
12. Check keyboard navigation, focus order, labels, and error announcement behavior.
13. Capture screenshots or describe visible evidence when useful.
14. Record pass/warning/fail findings with reproduction steps.
15. If verification reveals a missing API/domain/event rule, route back to the relevant backend or feature skill.

## Viewport baseline

Use project-specific viewport rules when available. Otherwise check at least:

```text
mobile: 390x844
small desktop: 1280x800
wide desktop: 1440x900
```

For dense dashboards, tables, or admin tools, also check horizontal overflow and compact widths.

## Browser evidence

Prefer direct browser verification when the project can run locally.

Useful evidence includes:

- route and viewport checked
- screenshot or visual description
- console error summary
- network/API status summary
- state scenario checked
- reproduction steps for findings

If browser verification cannot be run, say why and provide the strongest alternative check available.

## Output format

```text
Frontend Verification Report

Target:
- route/screen/component:
- user goal:
- changed files:

Environment:
- dev server:
- URL:
- browser/tool:
- data/mocks:

Scenarios checked:
- scenario:
- result:

Viewports checked:
- viewport:
- result:

Runtime health:
- console:
- network:
- assets/hydration:

Accessibility:
- keyboard:
- focus:
- labels/semantics:
- error announcements:

Findings:
- severity: source/screen -> issue -> reproduction -> suggested fix

Verification gaps:
- gap: why it remains

Status:
- pass | warning | fail

Next action:
- recommended follow-up
```

## Severity

```text
P0 fail:
  The UI cannot complete the core workflow, corrupts data, blocks users, or has severe runtime errors.

P1 warning:
  The UI works but has meaningful UX, accessibility, responsive, error-handling, or integration gaps.

P2 note:
  Minor polish, copy, spacing, or follow-up improvement.
```

## Pass criteria

A frontend verification can pass when:

- core user flow works
- relevant non-happy states are checked or intentionally out of scope
- no blocking console/network/runtime errors remain
- mobile and desktop layouts do not overlap, clip, or hide important actions
- forms prevent duplicate/invalid submission and show recoverable errors
- accessibility basics are acceptable for the touched UI
- backend/API assumptions are either confirmed or listed as gaps

## Anti-patterns

- Saying verification passed without opening the UI when a local browser check was possible.
- Checking only the desktop happy path.
- Ignoring console errors because the page looks fine.
- Treating mocked success data as proof that real API errors work.
- Forgetting disabled, unauthorized, empty, and validation states.
- Verifying screenshots while ignoring keyboard and focus behavior.
- Hiding unresolved API or backend assumptions in confident language.

## Example

```text
Frontend Verification Report

Target:
- route/screen/component: exhibition detail review form
- user goal: submit a review and see it in the list
- changed files: ReviewForm, ReviewSection, review API hook

Environment:
- dev server: npm run dev
- URL: http://localhost:3000/exhibitions/123
- browser/tool: Chromium
- data/mocks: logged-in user with no existing review

Scenarios checked:
- happy path submit: pass
- duplicate review error: warning, backend error shape not confirmed
- validation error for empty content: pass
- unauthorized user: pass

Viewports checked:
- 390x844: pass, no clipped submit button
- 1440x900: pass

Runtime health:
- console: no errors
- network: POST /reviews returns 201 in mock
- assets/hydration: no hydration warning observed

Accessibility:
- keyboard: rating and submit reachable
- focus: warning, focus does not return to trigger after modal close
- labels/semantics: pass
- error announcements: not verified

Findings:
- P1 warning: review modal -> focus is not restored after close -> reproduce by tabbing to Write Review, open modal, close with Escape -> restore focus to trigger button

Verification gaps:
- duplicate review API error code not confirmed with backend

Status:
- warning

Next action:
- fix modal focus restoration and confirm duplicate error contract
```