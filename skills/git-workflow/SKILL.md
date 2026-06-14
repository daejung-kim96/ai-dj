# Skill: Git Workflow

Use this skill when preparing commits, branches, or pull requests.

## Commit message rule

Use Korean summaries by default so the user can scan project history easily.

Keep concise conventional commit prefixes when useful for structure:

```text
type(scope): 한국어 요약
```

Use English type names such as `feat`, `fix`, `docs`, `chore`, and `refactor` unless the project profile says otherwise.

Examples:

```text
feat(context-router): 컨텍스트 라우팅 스킬 확장
docs(concepts): 공부용 개념 문서 한국어 기준 추가
fix(evals): 검증 체크리스트 누락 항목 보완
```

## Steps

1. Inspect changed files.
2. Group related changes.
3. Summarize behavior, not just files.
4. Mention verification.
5. Avoid committing secrets or generated noise.
