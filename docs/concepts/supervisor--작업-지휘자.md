# Supervisor (작업 지휘자)

Supervisor는 AI 하네스에서 작업 전체를 지휘하는 역할이다.

개별 스킬이 특정 일을 잘하는 도구라면, Supervisor는 어떤 스킬을 어떤 순서로 쓸지 결정하는 진행자다.

## 왜 필요한가

지금까지 만든 기반 스킬은 각각 역할이 다르다.

```text
context-router
  무엇을 읽을지 고른다.

generate-feature-context
  읽은 내용을 작업 가능한 문맥으로 묶는다.

verify-context-integrity
  결과가 근거와 맞는지 확인한다.

update-living-documentation
  오래 남길 결정과 학습을 문서로 기록한다.
```

하지만 사용자는 보통 이렇게 말하지 않는다.

```text
"context-router를 실행하고, feature context를 만들고, integrity를 검증한 다음 living docs를 업데이트해줘."
```

대신 이렇게 말한다.

```text
"로그인 기능 만들어줘."
"이 구조 괜찮은지 봐줘."
"다음에 뭐 하면 좋을까?"
```

Supervisor는 이런 자연어 요청을 보고 필요한 스킬 체인으로 바꾼다.

## 기본 흐름

```text
사용자 요청
-> 목표를 한 문장으로 정리
-> 프로젝트 프로필 확인
-> 요청 유형 분류
-> 필요한 스킬 체인 선택
-> 각 스킬을 순서대로 실행
-> 검증
-> 필요한 경우 문서 업데이트
-> 다음 행동 제안
```

## 좋은 Supervisor의 기준

좋은 Supervisor는 일을 크게 벌이지 않는다.

- 필요한 만큼만 읽는다.
- 가장 작은 유용한 스킬 체인을 고른다.
- 모르면 추측하지 않고 질문이나 누락 컨텍스트로 남긴다.
- 작업을 완료하기 전에 검증한다.
- 오래 남길 결정만 문서화한다.

## 스킬 체인의 예

기능 구현 요청:

```text
context-router
-> generate-feature-context
-> testing-tdd
-> backend-ddd 또는 frontend-feature
-> verify-context-integrity
-> update-living-documentation
```

이벤트 기반 백엔드 요청:

```text
context-router
-> generate-feature-context
-> testing-tdd
-> backend-ddd
-> backend-eda
-> verify-context-integrity
```

개념 학습 요청:

```text
context-router
-> docs/concepts 업데이트
-> verify-context-integrity
-> update-living-documentation
```

아키텍처 검토 요청:

```text
context-router
-> generate-feature-context 또는 관련 규칙 요약
-> verify-context-integrity
-> update-living-documentation
```

## ai-dj에서의 역할

`skills/supervisor`는 ai-dj의 기본 작업 루프를 묶는다.

나중에 프로젝트 프로필이 생기면 Supervisor가 먼저 프로젝트를 파악하고, 그 프로젝트의 파일 맵과 규칙에 맞는 스킬 체인을 고르게 된다.

## 기억할 문장

Supervisor는 "사용자 요청을 실행 가능한 AI 작업 순서로 바꾸는 지휘자"다.
