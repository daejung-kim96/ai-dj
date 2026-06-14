# 2026-06-14: Living Documentation v0.1

## Summary

Expanded `skills/update-living-documentation` and added a Korean study note for Living Documentation.

## Added

- `docs/concepts/living-documentation--살아있는-문서.md`
- durable knowledge categories
- target document placement rules
- entry templates
- quality checklist
- anti-patterns
- evaluation checklist item

## Why

The harness now has routing, feature context generation, and context integrity verification. It also needs a repeatable way to turn verified decisions and lessons into durable documentation.

## Next

The next foundation skill should be `supervisor`, because it can coordinate the full chain:

```text
context-router
-> generate-feature-context
-> verify-context-integrity
-> update-living-documentation
```
