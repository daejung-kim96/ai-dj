# ArtMoa Known Issues And Watch Points

## Transitional Backend Structure

Some contexts use the new folder names but may still contain older dependency
patterns. For example, an application service may still know presentation DTOs
or a domain repository may directly extend `JpaRepository`.

When refactoring, start with the smallest useful dependency cleanup:

1. Stop application services from returning presentation response DTOs.
2. Stop application services from accepting presentation request DTOs.
3. Introduce application command/result types only when they reduce confusion.
4. Split JPA entity and domain model later when the benefit is clear.

## Authentication Integration

Login and JWT work has been merged into backend `dev`. Older board APIs may
still have mock or transitional user-id handling. Before modifying board
authorization, inspect the current controller and security configuration.

## Object Storage

Production object storage is not finalized in this profile. Do not hardcode a
provider-specific SDK unless the infra repository or user request confirms the
provider and access pattern.

## Map Provider Setup

Kakao JavaScript SDK requires:

- JavaScript key in frontend environment variables
- JavaScript SDK domains registered in Kakao Developers
- Kakao Map product/API enabled in Kakao Developers

If a local map does not render, check all three before changing code.

## Secrets

Never commit real OAuth, Kakao, database, Redis, or object storage secrets.
Use `.env.example` for keys and `.env.local` or local environment files for
developer-specific values.
