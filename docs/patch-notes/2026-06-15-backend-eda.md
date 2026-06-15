# 2026-06-15: Backend EDA v0.1

## Summary

Expanded `skills/backend-eda` into a reusable event-driven backend workflow and added a Korean study note for Event-Driven Architecture.

## Added

- `docs/concepts/event-driven-architecture--이벤트-주도-아키텍처.md`
- EDA goal, triggers, inputs, and required context
- core concept distinctions for domain events, integration events, producers, consumers, idempotency, outbox, retry, and dead-letter handling
- event naming rules
- DDD and TDD connection points
- backend EDA output template
- quality checklist and anti-patterns
- evaluation checklist item for EDA usage

## Updated

- `skills/supervisor` event-driven backend chain now includes `backend-ddd` before `backend-eda` by default.
- `docs/concepts/skill-roadmap--스킬-로드맵.md` marks `backend-eda` as complete.

## Why

The harness needs a common way to design asynchronous workflows without hiding delivery risk. EDA keeps producers, consumers, transaction boundaries, idempotency, retries, and reprocessing visible before implementation.

## Next

Continue with `frontend-feature`, because the backend engineering workflows now cover test-first behavior, domain modeling, and event-driven integration.
