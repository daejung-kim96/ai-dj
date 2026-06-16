# ArtMoa Decisions

Record durable project decisions here. Keep this profile aligned with the
repository-local docs. If this file conflicts with an official repo doc, the
repo doc wins and this file should be updated.

## Harness Priority

Decision:

1. Current user request
2. Repository-local official docs and conventions
3. ArtMoa project profile
4. Common AI-DJ harness rules

Reason:

Project repositories are the source of truth for code-level conventions. The
AI-DJ profile should route and preserve context, not override the team's docs.

## Git Workflow

Decision:

- Feature work starts from `main` unless the user says otherwise.
- Work branches use `feature/<name>` for feature additions.
- Commit messages use a Conventional Commit prefix and Korean summary.
- Prefer small commits that show the work sequence.
- Documentation is updated continuously as living documentation.
- The usual ArtMoa merge flow is feature branch -> `main` -> `dev`.
- Before pull, merge, commit, or push, inspect branch status and avoid reverting
  user-owned changes.

Reason:

The user wants Git history to show what happened step by step, not only the final
large diff.

## Backend Architecture

Decision:

- The backend is moving toward a bounded-context-first modular monolith.
- Official backend rules live in:
  - `/home/dvkim96/workspace/artmoa/artmoa-backend-next/docs/backend-convention.md`
  - `/home/dvkim96/workspace/artmoa/artmoa-backend-next/docs/backend-package-structure.md`
  - `/home/dvkim96/workspace/artmoa/artmoa-backend-next/docs/db-convention.md`
- Identity is the current reference implementation for stronger DDD and
  hexagonal separation.
- Board, Attachment, and Venue have been moved toward the target package layout,
  but some code may still be transitional.

Reason:

Team development is in progress, so the harness should support gradual
conversion instead of assuming every context is fully refactored.

## Map And Venue

Decision:

- Kakao is the preferred map provider for the current ArtMoa implementation.
- Venue is managed as a reusable place concept instead of storing only raw
  coordinates on each exhibition.
- Frontend map markers should eventually render exhibitions through their linked
  venues.

Reason:

Reusable Venue records reduce duplicate geocoding, keep coordinates consistent,
and support both crawled exhibitions and artist-entered exhibitions.

## Attachments And Rich Content

Decision:

- For rich board content, store attachment references in content using stable
  attachment IDs rather than treating object URLs as permanent content.
- Backend responses can provide renderable URLs for those attachments.
- Local development may route file downloads through the backend.
- Production object storage can provide direct public, CDN, or signed URLs when
  infrastructure is ready.

Reason:

Attachment IDs keep content stable across storage provider changes, URL expiry,
CDN migration, and access-control changes.
