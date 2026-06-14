# Skill: Verify Context Integrity

Use this skill to check whether generated work matches the selected context.

## Checks

- Required files were read.
- Named features exist in specs or are clearly new.
- API names and permissions match source docs.
- State values are consistent.
- Project-specific rules were not moved into common skills.
- Durable decisions were recorded.

## Output

```text
Status: pass | warning | fail
Findings:
- severity: source -> issue -> suggested fix
```
