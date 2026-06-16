# Project Profiles

Each project profile adapts the common harness to one project.

Common cross-project rules stay in `AGENT.md`, `docs/rules/`, and `skills/`.
Project profiles should reference those common rules instead of copying them.

Recommended structure:

```text
project-profiles/<project>/
  project.yaml
  required-reading-map.md
  domain-terms.md
  decisions.md
  known-issues.md
```

Keep service-specific facts here instead of hardcoding them into common skills.

## Active Profiles

| Project | Profile | Purpose |
| --- | --- | --- |
| ArtMoa | `project-profiles/artmoa/` | Routes ArtMoa backend, frontend, infra, map, board, attachment, venue, and Git workflow requests. |
