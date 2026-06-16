# ArtMoa Required Reading Map

Use this map to decide what to read before acting. Start narrow and expand only
when the first pass reveals missing context.

## Always Read First

- `project-profiles/artmoa/project.yaml`
- `project-profiles/artmoa/decisions.md`
- `project-profiles/artmoa/known-issues.md`

## Backend Requests

Repository: `/home/dvkim96/workspace/artmoa/artmoa-backend-next`

Read these first:

- `docs/backend-convention.md`
- `docs/backend-package-structure.md`
- `docs/db-convention.md`
- Relevant source files under `src/main/java/com/artmoa/<context>/`
- Relevant tests under `src/test/java/com/artmoa/<context>/`

Request routing:

- Board, free posts, suggestions, comments: `src/main/java/com/artmoa/board/`
- Attachments and image upload/download: `src/main/java/com/artmoa/attachment/`
- Login, OAuth, JWT, current user: `src/main/java/com/artmoa/identity/`
- Venue, map API, Kakao local search: `src/main/java/com/artmoa/venue/`
- DB schema changes: `src/main/resources/db/migration/`

## Frontend Requests

Repository: `/home/dvkim96/workspace/artmoa/artmoa-frontend-next`

Read these first:

- `package.json`
- `src/app/`
- `src/features/`
- `src/shared/`
- Relevant docs under `docs/`

Request routing:

- Map page and Kakao SDK: `src/app/map/`, `src/features/venue-map/`
- Shared layout: `src/shared/ui/`
- Environment variables: `.env.local.example`, `.env.local`

Never copy secrets from `.env.local` into docs or commits.

## Infrastructure Requests

Repository: `/home/dvkim96/workspace/artmoa/artmoa-infra-next`

Read deployment and compose files before recommending local setup changes. Keep
service version assumptions grounded in checked files.

## Git And Release Requests

Read:

- Current `git status --short --branch`
- Recent `git log --oneline --decorate`
- Project-specific branch flow in `decisions.md`

Before committing and pushing, follow `skills/git-workflow/SKILL.md`.

## Learning Or Explanation Requests

When the user asks for concepts, compare the concept against the current ArtMoa
codebase when useful, then write the explanation in Korean. If it should be
reused, add or update a document under `docs/concepts/` or the active project
profile.
