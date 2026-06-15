# Skill: Backend EDA

Use this skill for event-driven backend work involving domain events, integration events, producers, consumers, message payloads, idempotency, retries, dead-letter queues, and outbox patterns.

## Goal

Turn backend behavior into a reliable event-driven workflow with clear event ownership, delivery boundaries, idempotent consumers, and observable failure handling.

This skill should make asynchronous behavior explicit instead of hiding it inside side effects.

## When to use

Use this skill when the user asks to:

- design or implement event-driven backend behavior
- publish events after domain or application state changes
- define producers, consumers, topics, queues, or handlers
- design idempotency, retries, dead-letter behavior, or reprocessing
- separate domain events from integration events
- review whether event payloads and transaction boundaries are safe
- connect DDD aggregate decisions to asynchronous workflows

Do not use this skill for simple in-process method calls when no asynchronous boundary, integration boundary, or eventual consistency is needed.

## Inputs

- User request, issue, or feature context pack
- TDD plan from `skills/testing-tdd`, if behavior has been defined
- DDD plan from `skills/backend-ddd`, if domain boundaries are involved
- Existing event contracts, topics, queues, message schemas, handlers, and tests
- Active project profile, if any
- Existing retry, idempotency, outbox, observability, and error-handling conventions

## Required context

At minimum, identify:

- behavior or state change that triggers the event
- source aggregate or application service
- event name and whether it is a domain event or integration event
- producer and transaction boundary
- event payload and schema/version
- consumers and their responsibilities
- idempotency key and duplicate handling
- ordering expectations
- retry strategy and dead-letter handling
- observability fields and correlation IDs
- tests that prove publishing, consuming, idempotency, and failure behavior

If any required context is missing, list it as an open question instead of inventing it.

## Core distinctions

| Concept | Use it for |
| --- | --- |
| Event-driven architecture | Systems communicating through events rather than direct synchronous calls |
| Domain event | A fact that happened inside one bounded context |
| Integration event | A published contract for other systems or contexts |
| Producer | The code or service that publishes an event |
| Consumer | The code or service that reacts to an event |
| Topic/queue | The transport channel for messages |
| Idempotency key | A stable key used to safely ignore duplicate work |
| Outbox pattern | Storing event records in the same transaction as state changes, then publishing later |
| Retry policy | Rules for trying failed work again |
| Dead-letter queue | A place for messages that could not be processed safely |
| Eventual consistency | Accepting temporary inconsistency while asynchronous work catches up |

## Event naming rules

Name events as facts that already happened.

Prefer:

```text
ReviewCreated
PaymentCaptured
ExhibitionApproved
NotificationDeliveryFailed
```

Avoid command-like names:

```text
CreateReview
CapturePayment
ApproveExhibition
SendNotification
```

Use project naming conventions when a project profile or existing event contracts define them.

## Workflow

1. Restate the asynchronous workflow in one sentence.
2. List source references, existing event contracts, handlers, and tests actually read.
3. Confirm the domain or application state change that triggers the event.
4. Decide whether the event is a domain event, integration event, or both.
5. Name the event as a past-tense fact.
6. Identify the producer and the transaction boundary.
7. Decide whether the outbox pattern is needed to avoid losing events after state changes.
8. Define payload fields, schema version, event ID, occurred-at time, and correlation ID.
9. Define consumers, side effects, and ownership of each reaction.
10. Define idempotency keys and duplicate handling for every consumer.
11. Define ordering expectations and what happens when messages arrive late or out of order.
12. Define retry, backoff, dead-letter, alerting, and reprocessing behavior.
13. Add or update tests for publish, consume, idempotency, retry, and dead-letter behavior.
14. Verify event contracts against source context and project conventions.

## DDD connection

Use `skills/backend-ddd` before this skill when the event comes from domain behavior or aggregate state changes.

Use this skill after the DDD plan to decide:

- which aggregate or application service produces the event
- which invariant must be satisfied before publishing
- whether the event is internal to a bounded context or crosses a context boundary
- which event payload is safe to expose
- which consumers should react without taking ownership of the producer's domain rules

If EDA modeling reveals a missing invariant or unclear aggregate ownership, return to `skills/backend-ddd` before implementing the event.

## TDD connection

Use `skills/testing-tdd` before this skill when event behavior is not already protected by tests.

Important test cases usually include:

- event is published after the successful state change
- event is not published when the command fails
- consumer handles duplicate messages safely
- consumer can retry transient failures
- poison messages go to dead-letter handling
- reprocessing does not corrupt state

## Output format

```text
Backend EDA Plan

Workflow:
- summary:
- source:

Event:
- name:
- type: domain event | integration event
- reason:
- version:

Producer:
- service/module:
- trigger:
- transaction boundary:
- outbox needed:

Payload:
- fields:
- idempotency key:
- correlation/trace fields:
- sensitive fields excluded:

Consumers:
- consumer:
- responsibility:
- side effects:
- idempotency handling:

Delivery:
- topic/queue:
- ordering expectation:
- retry policy:
- dead-letter behavior:
- reprocessing plan:

Tests:
- publish test:
- consume test:
- idempotency test:
- failure/retry test:

Open questions:
- question:

Risks:
- risk: mitigation
```

## Quality checklist

- The event represents a fact that already happened.
- Event type is clear: domain event, integration event, or both with separate contracts.
- Producer and transaction boundary are explicit.
- State change and event publication cannot silently diverge.
- Payload is versioned and contains only safe, necessary fields.
- Consumers do not rely on producer internals.
- Idempotency is defined for every consumer with side effects.
- Retry, dead-letter, and reprocessing behavior are defined.
- Ordering expectations are explicit instead of assumed.
- Tests cover publish, consume, duplicates, and failure behavior.
- Project-specific topics, payloads, and provider details stay in project profiles or source references.

## Anti-patterns

- Naming events like commands.
- Publishing events before the state change is safely committed.
- Assuming at-most-once or exactly-once delivery without project evidence.
- Letting consumers mutate producer-owned domain state directly.
- Sending entire database rows as event payloads.
- Ignoring idempotency because "the broker should not duplicate messages."
- Retrying forever without dead-letter or alerting.
- Treating asynchronous eventual consistency as if it were immediate consistency.

## Example

```text
Backend EDA Plan

Workflow:
- summary: After a review is created, notify interested users asynchronously.
- source: docs/specs/reviews.md

Event:
- name: ReviewCreated
- type: integration event
- reason: notification is handled outside the review use case boundary
- version: v1

Producer:
- service/module: review application service
- trigger: review creation transaction succeeds
- transaction boundary: save review and outbox record together
- outbox needed: yes, to avoid losing the event after saving the review

Payload:
- fields: eventId, reviewId, itemId, authorId, rating, occurredAt
- idempotency key: eventId
- correlation/trace fields: correlationId
- sensitive fields excluded: review content body if notification does not need it

Consumers:
- consumer: notification worker
- responsibility: create notification request
- side effects: enqueue push/email notification
- idempotency handling: ignore eventId already processed

Delivery:
- topic/queue: review-events
- ordering expectation: no strict ordering required for notification
- retry policy: retry transient failures with backoff
- dead-letter behavior: move poison messages to DLQ and alert
- reprocessing plan: replay DLQ after fixing handler or data issue

Tests:
- publish test: ReviewCreated is recorded in outbox after successful review creation
- consume test: notification worker creates one notification request
- idempotency test: duplicate eventId does not create duplicate notifications
- failure/retry test: transient notification error is retried; invalid payload goes to DLQ

Open questions:
- Should review updates also publish events, or only review creation?
```
