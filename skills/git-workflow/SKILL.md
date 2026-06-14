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
5. Propose a Korean commit message.
6. Show the target remote and branch.
7. Stop and ask the user whether to commit and push.
8. After approval, commit and push as one continuous Git publishing step.
9. Avoid committing secrets or generated noise.

## Pre-publish review

Before running `git commit` and `git push`, show:

```text
Changed files:
- ...

Change summary:
- ...

Verification:
- ...

Proposed commit message:
- type(scope): 한국어 요약

Remote and branch:
- origin main

Question:
- 이 내용으로 커밋하고 push할까요?
```

## Approval rule

Commit and push use one approval point by default.

Do not split commit and push into separate confirmations unless the user asks for that workflow.
