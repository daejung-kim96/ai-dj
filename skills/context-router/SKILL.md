# Skill: Context Router

Use this skill to decide which files an agent must read before acting.

## Inputs

- User request
- Active project profile
- Available source map
- Selected skill

## Steps

1. Identify keywords, domain objects, and task type.
2. Read the project `required-reading-map.md` if a profile exists.
3. Select only the files needed for the first pass.
4. Mark optional files separately.
5. Expand context only when blocked or when verification requires it.

## Output format

```text
Required:
- path: reason

Optional:
- path: reason

Missing:
- expected path or source: why it matters
```
