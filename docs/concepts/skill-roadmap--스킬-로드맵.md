# Skill Roadmap (스킬 로드맵)

이 문서는 ai-dj의 공통 스킬을 어떤 순서로 만들어가면 좋은지 정리한다.

## 1단계: 기반 스킬

1. `context-router`: 작업 전에 어떤 파일을 읽을지 결정한다. 완료
2. `generate-feature-context`: 선택된 파일들을 구현 가능한 컨텍스트 팩으로 정리한다. 완료
3. `verify-context-integrity`: 결과가 선택한 컨텍스트와 맞는지 확인한다. 완료
4. `update-living-documentation`: 오래 남길 학습 내용과 결정을 기록한다. 완료
5. `supervisor`: 넓은 요청에서 여러 스킬의 순서를 조율한다. 완료

## 2단계: 엔지니어링 워크플로우

1. `testing-tdd`: 요구사항을 먼저 테스트로 바꾼다. 완료
2. `backend-ddd`: 도메인, 애그리거트, 불변식, 리포지토리를 모델링한다. 완료
3. `backend-eda`: 이벤트, 생산자, 소비자, 멱등성을 모델링한다. 완료
4. `frontend-feature`: 화면, 컴포넌트, 폼, 상태, API 연동을 만든다. 완료
5. `frontend-verification`: 실제 브라우저에서 화면 상태, 반응형, 접근성, 콘솔 오류를 검증한다. 완료
6. `git-workflow`: 커밋과 PR 요약을 준비한다.

## 3단계: 프로젝트 프로필

공통 스킬이 어느 정도 안정되면 프로젝트 프로필을 추가한다.

예시:

```text
project-profiles/artmoa/
  project.yaml
  required-reading-map.md
  domain-terms.md
  decisions.md
  known-issues.md
```

## 판단 기준

미래의 혼란을 줄이는 순서로 스킬을 만든다.

그래서 `context-router`가 먼저다. 다른 스킬을 쓰기 전에 에이전트가 무엇을 알아야 하는지 결정하기 때문이다.

## 현재 기본 루프

```text
supervisor
-> context-router
-> generate-feature-context
-> task-specific skill
-> verify-context-integrity
-> update-living-documentation
```
