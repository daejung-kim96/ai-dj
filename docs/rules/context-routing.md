# Context Routing Rule

The context router decides which files to read for a request.

## Purpose

Context routing keeps AI work grounded without making every request read the whole repository.

## Inputs

- User request
- Active project profile
- Skill requirements
- Known source map

## Output

A short list of files to read, grouped by purpose and importance.

## Routing categories

- Product/spec request
- Backend request
- Frontend request
- Data pipeline request
- Testing request
- Documentation request
- Git/release request
- Architecture review
- Troubleshooting request
- Learning/concept request

## Core rule

Start narrow. Expand only when the first pass reveals missing context.

## Required, optional, missing

- Required: files that must be read before acting.
- Optional: files that may improve quality but should not block the first pass.
- Missing: expected context that is unavailable and should be treated as risk.

## Source priority

Use this priority order when several files seem relevant:

1. Active project profile.
2. Source-of-truth specs and contracts.
3. Current source code.
4. Tests and fixtures.
5. Architecture decisions and patch notes.
6. Examples and generated reports.

## Anti-patterns

- Reading every file by default.
- Using a stale summary when a source file is available.
- Mixing project-specific facts into common skills.
- Acting on a domain assumption without checking the project profile.
- Ignoring missing context because the model can guess.
