# 2026-06-21: Frontend Verification v0.1

## Summary

Added `skills/frontend-verification` as a reusable frontend browser verification workflow and added a Korean study note for frontend verification.

## Added

- `skills/frontend-verification/SKILL.md`
- `docs/concepts/frontend-verification--프론트엔드-검증.md`
- frontend verification dimensions for behavior, UI states, API integration, visual fit, responsive layout, accessibility, runtime health, and regression risk
- browser evidence and viewport baseline guidance
- verification report output template
- severity levels and pass criteria
- evaluation checklist item for frontend verification usage

## Updated

- `skills/supervisor` frontend work chain now runs `frontend-verification` after `frontend-feature`.
- `AGENT.md` and `README.md` include the new common skill.
- concept index, glossary, skill roadmap, and skills study note reference frontend verification.

## Why

The harness needs a frontend verification loop that checks real browser behavior after implementation. This helps catch issues that code review misses: console errors, broken states, text clipping, responsive regressions, focus problems, and unconfirmed backend assumptions.

## Next

Consider `frontend-api-contract` next if frontend/backend integration gaps keep appearing during verification.