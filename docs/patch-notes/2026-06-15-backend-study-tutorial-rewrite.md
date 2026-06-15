# 2026-06-15: Backend Study Tutorial Rewrite

## Summary

Rewrote the combined backend TDD-DDD-EDA study note as a step-by-step tutorial instead of a compact concept summary.

## Changed

- Replaced the short integrated flow explanation with a review-feature tutorial.
- Added a deliberately weak implementation path to show why the concepts are needed.
- Expanded TDD with behavior tables, spoken test cases, and Red-Green-Refactor applied to duplicate reviews.
- Expanded DDD with domain language, bounded contexts, entity/value object distinctions, aggregate decisions, and application service responsibilities.
- Expanded EDA with synchronous-call failure modes, event naming, outbox, idempotent consumers, retry, DLQ, and reprocessing.
- Added AI prompt examples and self-check checklists.

## Why

The previous study note added examples, but still read like a summary. This rewrite is meant to be easier to learn from because it follows one feature from vague requirement to test, domain model, and event workflow.
