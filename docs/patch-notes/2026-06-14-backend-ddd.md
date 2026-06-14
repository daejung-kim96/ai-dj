# 2026-06-14: Backend DDD v0.1

## Summary

Expanded `skills/backend-ddd` into a reusable domain modeling workflow and added a Korean study note for Domain-Driven Design.

## Added

- `docs/concepts/domain-driven-design--도메인-주도-설계.md`
- DDD goal, triggers, inputs, and required context
- core concept distinctions for bounded context, aggregate, invariant, repository, and services
- TDD connection for turning behavior tests into domain model decisions
- backend DDD output template
- quality checklist and anti-patterns
- evaluation checklist item for DDD usage

## Updated

- `skills/supervisor` now places `testing-tdd` before engineering implementation skills by default.
- `docs/concepts/skill-roadmap--스킬-로드맵.md` marks `backend-ddd` as complete.

## Why

The harness needs a common way to prevent backend work from becoming table-driven or framework-driven. DDD keeps business language, invariants, state transitions, and ownership rules visible before implementation.

## Next

Continue with `backend-eda`, because event-driven workflows can build on DDD decisions about aggregates, domain events, and consistency boundaries.
