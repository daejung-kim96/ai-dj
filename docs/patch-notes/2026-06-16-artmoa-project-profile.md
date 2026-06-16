# 2026-06-16 ArtMoa Project Profile

## Summary

Added an ArtMoa project profile so AI-DJ can keep common harness rules separate
from ArtMoa-specific workflow and repository context.

## Changes

- Added `project-profiles/artmoa/project.yaml`.
- Added ArtMoa required reading map.
- Added ArtMoa domain terms.
- Added durable ArtMoa decisions.
- Added known issues and watch points.
- Updated `project-profiles/README.md` with the active ArtMoa profile.

## Intent

Common harness behavior remains in `AGENT.md`, `docs/rules/`, and `skills/`.
ArtMoa-specific paths, branch flow, DDD transition notes, map decisions, and
attachment decisions now live in `project-profiles/artmoa/`.
