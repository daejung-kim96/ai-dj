# Frontend Verification (프론트엔드 검증)

English name: Frontend Verification
한국어 이름: 프론트엔드 검증
쉽게 말하면: 만든 화면이 실제 브라우저에서 사용자 흐름, 상태, 반응형, 접근성, API 연동까지 제대로 동작하는지 확인하는 과정

## 핵심 생각

프론트엔드는 코드가 컴파일된다고 끝나지 않는다.

사용자는 실제 브라우저에서 화면을 보고, 클릭하고, 입력하고, 실패를 만나고, 모바일에서 스크롤하고, 키보드로 이동한다. 그래서 프론트 검증은 코드만 보는 것이 아니라 실제 경험을 확인해야 한다.

```text
화면이 뜨는가?
버튼이 눌리는가?
저장 중에는 중복 제출이 막히는가?
오류가 나면 회복할 수 있는가?
모바일에서 버튼이 잘리지 않는가?
콘솔 에러가 없는가?
키보드로 조작할 수 있는가?
```

이 질문을 확인하는 과정이 프론트엔드 검증이다.

## 왜 별도 스킬이 필요한가

`frontend-feature`는 화면을 어떻게 설계하고 만들지 정리한다.

`frontend-verification`은 만든 화면이 실제로 괜찮은지 확인한다.

둘은 역할이 다르다.

| 단계 | 보는 것 |
| --- | --- |
| Frontend Feature | 사용자 흐름, 상태 모델, 컴포넌트 경계, API 계약 설계 |
| Frontend Verification | 브라우저에서 실제 동작, 화면 깨짐, 콘솔 오류, 접근성, 반응형 확인 |

AI는 종종 코드를 그럴듯하게 만들고 검증을 건너뛴다. 그래서 하네스에 검증 루프를 따로 둔다.

## 예시: 리뷰 작성 폼 검증

리뷰 작성 폼을 만들었다고 하자.

성공 화면만 보면 괜찮아 보일 수 있다.

```text
평점 선택 가능
내용 입력 가능
등록 버튼 있음
리뷰 목록 표시됨
```

하지만 검증은 더 넓게 본다.

```text
로그인하지 않은 사용자는 작성 폼 대신 로그인 안내를 보는가?
이미 리뷰를 작성한 사용자는 중복 작성 버튼을 볼 수 없는가?
평점을 선택하지 않으면 등록 버튼이 비활성화되는가?
내용이 비어 있으면 필드 오류가 보이는가?
저장 중 버튼이 다시 눌리지 않는가?
서버가 409 duplicate review를 반환하면 사용자가 이해할 수 있는가?
모바일에서 textarea가 화면 밖으로 밀리지 않는가?
콘솔에 runtime error가 없는가?
Escape로 모달을 닫으면 focus가 원래 버튼으로 돌아가는가?
```

이런 것들이 실제 사용 품질을 만든다.

## 검증해야 하는 상태들

프론트 검증은 happy path만 보지 않는다.

| 상태 | 확인 질문 |
| --- | --- |
| Loading | 데이터가 올 때까지 이상한 빈 화면이 아닌가? |
| Empty | 데이터가 없을 때 사용자가 다음 행동을 알 수 있는가? |
| Error | 실패 이유와 회복 방법이 보이는가? |
| Success | 성공 후 화면이 예상 상태로 갱신되는가? |
| Disabled | 버튼이 비활성화되는 이유가 자연스러운가? |
| Unauthorized | 로그인/권한 안내가 적절한가? |
| Validation | 필드 오류가 정확한 위치에 보이는가? |
| Pending | 저장 중 중복 클릭이 막히는가? |
| Retry | 실패 후 다시 시도할 수 있는가? |

이 중 어떤 상태가 기능에 필요한지 먼저 정하고, 실제 화면에서 확인한다.

## 브라우저에서 봐야 하는 것

프론트 검증에서는 브라우저가 중요하다.

코드만 봐서는 다음 문제를 놓치기 쉽다.

```text
텍스트가 버튼 밖으로 삐져나감
모바일에서 하단 버튼이 가려짐
긴 제목 때문에 카드 높이가 깨짐
API 에러가 콘솔에만 찍히고 화면에는 안 보임
hydration warning이 발생함
이미지가 404임
모달 focus가 뒤 페이지로 빠짐
```

그래서 가능하면 로컬 dev server를 띄우고 직접 확인한다.

기본 viewport 예시:

```text
mobile: 390x844
small desktop: 1280x800
wide desktop: 1440x900
```

프로젝트에 별도 기준이 있으면 그 기준을 따른다.

## Console과 Network도 검증 대상이다

화면이 멀쩡해 보여도 콘솔에 에러가 있으면 문제가 숨어 있을 수 있다.

확인할 것:

```text
console error
console warning 중 실제 버그 가능성이 있는 것
network 4xx/5xx
asset 404
hydration mismatch
CORS error
API response shape mismatch
```

특히 API 연동 직후에는 network tab이 중요하다. UI가 mock 데이터로만 맞아 보이고 실제 API shape과 안 맞을 수 있기 때문이다.

## Accessibility (접근성) 검증

접근성은 자동 도구만으로 충분하지 않다.

최소한 다음은 직접 확인한다.

```text
Tab 키로 주요 조작이 가능한가?
focus 순서가 자연스러운가?
모달을 열면 focus가 모달 안으로 들어가는가?
모달을 닫으면 focus가 원래 트리거로 돌아가는가?
input과 label이 연결되어 있는가?
오류 메시지가 어떤 필드 오류인지 알 수 있는가?
색만으로 상태를 구분하지 않는가?
```

프론트 검증에서 접근성을 매번 완벽히 감사할 필요는 없지만, touched UI의 기본 동작은 확인해야 한다.

## Verification Report (검증 보고서)

검증 결과는 그냥 "확인 완료"라고 쓰면 다음 작업에 도움이 안 된다.

좋은 보고서는 무엇을, 어떤 환경에서, 어떤 상태로 확인했는지 남긴다.

```text
Target:
  /exhibitions/123 리뷰 작성 폼

Environment:
  npm run dev
  Chromium
  390x844, 1440x900

Scenarios checked:
  로그인 사용자 리뷰 작성 성공
  평점 미선택 validation
  중복 리뷰 오류
  비로그인 사용자

Runtime health:
  console error 없음
  network 201/409 mock 확인

Findings:
  P1: 모바일에서 submit 버튼이 키보드에 가려짐

Status:
  warning
```

이렇게 남기면 다음 사람이 무엇을 믿어도 되는지 알 수 있다.

## 백엔드로 되돌아가야 하는 순간

검증 중에 프론트 문제가 아니라 API 계약 문제를 발견할 수 있다.

예시:

```text
중복 리뷰 오류가 409인지 400인지 불명확하다.
validation error response가 필드별인지 메시지 하나인지 모른다.
리뷰 작성 가능 여부를 확인하는 API가 없다.
이벤트 기반 후속 상태가 언제 UI에 반영되는지 모른다.
```

이런 경우 프론트에서 임의로 정하지 않는다.

```text
API 계약 문제 -> generate-feature-context 또는 backend-ddd
도메인 규칙 문제 -> backend-ddd
이벤트 반영 시점 문제 -> backend-eda
테스트 기준 문제 -> testing-tdd
```

## ai-dj에서 쓰는 방식

프론트 기능 작업의 기본 흐름은 다음과 같다.

```text
context-router
-> generate-feature-context
-> testing-tdd
-> frontend-feature
-> frontend-verification
-> verify-context-integrity
```

`frontend-verification`은 `frontend-feature`가 만든 계획이나 실제 구현 결과를 확인한다.

검증에서 발견한 문제는 세 갈래로 나뉜다.

```text
프론트 구현 문제:
  frontend-feature로 돌아가 수정한다.

백엔드 계약 문제:
  backend-ddd 또는 backend-eda로 되돌아간다.

문서/규칙 문제:
  update-living-documentation으로 기록한다.
```

## 기억할 문장

프론트엔드 검증은 "화면을 봤다"가 아니라, 사용자가 실제로 성공하고 실패에서 회복할 수 있는지 확인하는 일이다.