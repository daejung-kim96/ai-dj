# Frontend Feature Development (프론트엔드 기능 개발)

English name: Frontend Feature Development
한국어 이름: 프론트엔드 기능 개발
쉽게 말하면: 사용자가 실제로 기능을 완료할 수 있도록 화면, 상태, 폼, API, 접근성까지 함께 설계하는 일

## 핵심 생각

프론트엔드는 "화면을 예쁘게 만드는 영역"만이 아니다.

좋은 프론트 기능은 사용자가 목표를 완료하는 동안 필요한 모든 상태를 다룬다.

```text
처음 들어왔을 때
데이터를 불러오는 중일 때
데이터가 없을 때
권한이 없을 때
입력값이 틀렸을 때
저장 중일 때
저장에 실패했을 때
저장에 성공했을 때
모바일 화면에서 볼 때
키보드로 조작할 때
```

즉 프론트엔드 기능 개발은 "성공 화면 하나"가 아니라 사용자 흐름 전체를 설계하는 일이다.

## 백엔드와 다른 출발점

백엔드는 주로 규칙과 데이터 일관성을 본다.

프론트엔드는 사용자가 그 규칙을 어떻게 만나고, 이해하고, 회복하는지를 본다.

예를 들어 "중복 리뷰는 작성할 수 없다"는 백엔드 규칙이 있다고 하자.

백엔드 관점:

```text
같은 userId + exhibitionId 리뷰가 이미 있으면 DuplicateReview 오류를 반환한다.
```

프론트 관점:

```text
사용자가 이미 리뷰를 작성했다면 작성 버튼을 숨길 것인가?
버튼은 보이되 비활성화할 것인가?
이미 작성한 리뷰 수정으로 안내할 것인가?
서버에서 duplicate error가 오면 어떤 문구를 보여줄 것인가?
```

같은 규칙이라도 프론트에서는 사용자 경험으로 다시 번역해야 한다.

## 예시: 전시 리뷰 작성 폼

요구사항:

```text
로그인한 사용자는 전시에 리뷰를 작성할 수 있다.
평점은 1점부터 5점까지 선택한다.
내용은 필수다.
이미 리뷰를 작성한 사용자는 같은 전시에 다시 작성할 수 없다.
리뷰 작성에 성공하면 리뷰 목록에 새 리뷰가 보인다.
```

이걸 바로 컴포넌트로 만들면 이렇게 흐르기 쉽다.

```text
ReviewForm 컴포넌트 만든다.
rating input 만든다.
textarea 만든다.
submit 버튼 만든다.
POST /reviews 호출한다.
```

하지만 실제 기능에는 더 많은 질문이 있다.

```text
로그인하지 않은 사용자는 무엇을 보는가?
이미 리뷰를 쓴 사용자는 무엇을 보는가?
평점을 선택하지 않으면 submit 버튼은 어떤 상태인가?
내용이 비어 있으면 에러는 언제 보여주는가?
저장 중에는 버튼을 비활성화하는가?
저장 실패 후 사용자가 다시 시도할 수 있는가?
성공 후 폼을 닫는가, 초기화하는가, 작성된 리뷰로 이동하는가?
모바일에서 textarea와 버튼이 잘 보이는가?
키보드만으로 평점을 선택할 수 있는가?
```

프론트 스킬은 이런 질문을 빠뜨리지 않게 만드는 역할을 한다.

## User Flow (사용자 흐름)

먼저 컴포넌트가 아니라 사용자의 길을 본다.

```text
1. 사용자가 전시 상세 페이지에 들어온다.
2. 리뷰 작성 가능 여부를 확인한다.
3. 작성 가능하면 리뷰 폼을 연다.
4. 평점과 내용을 입력한다.
5. submit 버튼을 누른다.
6. 저장 중 상태를 본다.
7. 성공하면 새 리뷰가 목록에 보인다.
8. 실패하면 이유를 보고 수정하거나 다시 시도한다.
```

이 흐름이 없으면 컴포넌트는 만들었는데 사용자가 막히는 지점이 생긴다.

## Server State와 Client UI State

프론트 상태는 크게 두 가지로 나눠서 보면 편하다.

Server State (서버 상태):

```text
전시 정보
현재 로그인 사용자
이미 작성한 리뷰 여부
리뷰 목록
POST /reviews 응답
```

Client UI State (클라이언트 UI 상태):

```text
리뷰 폼이 열려 있는가
평점 draft 값
내용 draft 값
submit 중인가
어떤 필드에 에러가 보이는가
모달 포커스가 어디에 있는가
```

서버 상태와 UI 상태를 섞으면 화면이 복잡해진다.

예를 들어 `reviews` 목록은 서버에서 온 데이터다. 하지만 `textarea`에 아직 저장하지 않은 내용은 UI draft다. 둘을 같은 상태처럼 다루면 저장 실패나 취소 시 처리가 어려워진다.

## UI States (화면 상태)

좋은 프론트 기능은 성공 상태만 만들지 않는다.

리뷰 폼 예시에서 필요한 상태는 다음과 같다.

| 상태 | 화면에서 필요한 것 |
| --- | --- |
| Loading | 리뷰 가능 여부를 불러오는 중임을 보여준다 |
| Empty | 아직 리뷰가 없으면 빈 목록을 자연스럽게 보여준다 |
| Unauthorized | 로그인 안내를 보여준다 |
| Disabled | 입력이 불완전하거나 저장 중이면 버튼을 비활성화한다 |
| Validation Error | 평점 미선택, 내용 없음 같은 필드 오류를 보여준다 |
| Submit Pending | 중복 제출을 막고 저장 중임을 보여준다 |
| Submit Error | 서버 오류나 중복 리뷰 오류를 회복 가능한 메시지로 보여준다 |
| Success | 새 리뷰가 보이고 폼이 닫히거나 초기화된다 |

이 상태들을 먼저 적어두면 AI가 성공 화면 하나만 만드는 일을 줄일 수 있다.

## API Contract (API 계약)

프론트는 백엔드 API를 추측하면 안 된다.

리뷰 작성 폼이라면 최소한 다음 계약이 필요하다.

```text
Endpoint:
  POST /reviews

Request:
  exhibitionId
  rating
  content

Success response:
  reviewId
  rating
  content
  author
  createdAt

Errors:
  400 invalid rating
  401 unauthorized
  409 duplicate review
```

여기서 error code나 response shape이 없으면 프론트에서 임의로 만들지 않는다. 이때는 백엔드 쪽으로 되돌아가서 계약을 확정해야 한다.

ai-dj에서는 이런 경우를 `Backend follow-up`으로 남긴다.

```text
Backend follow-up:
- duplicate review error code가 409인지 확인 필요
- validation error response가 fieldErrors 형태인지 확인 필요
```

## Component Boundary (컴포넌트 경계)

컴포넌트를 나눌 때는 "작게 많이"보다 "책임이 선명하게"가 중요하다.

리뷰 폼 예시:

```text
ReviewSection
  리뷰 목록과 작성 가능 여부를 관리한다.

ReviewForm
  평점, 내용, submit 흐름을 관리한다.

RatingInput
  평점 선택 UI와 키보드 조작을 담당한다.

ReviewList
  리뷰 목록을 렌더링한다.

ReviewErrorMessage
  서버/검증 오류를 사용자 문구로 보여준다.
```

나쁜 방향:

```text
ExhibitionDetailPage 하나에
  데이터 fetch
  form state
  validation
  API submit
  review list rendering
  modal focus
  error mapping
전부 들어간다.
```

처음에는 빠르지만 조금만 기능이 늘어나도 읽기 어려워진다.

## Form Behavior (폼 동작)

폼은 입력 요소 몇 개가 아니다. 제출 전후의 생명주기가 있다.

```text
초기값:
  rating 없음
  content 빈 문자열

검증:
  rating 필수
  content 필수
  content 최대 길이

submit 가능 조건:
  로그인됨
  작성 가능함
  rating 유효
  content 유효
  submit 중 아님

submit 중:
  버튼 disabled
  중복 클릭 방지
  필요하면 spinner 표시

성공:
  새 리뷰 목록에 반영
  폼 초기화 또는 닫기

실패:
  필드 오류는 필드 옆에 표시
  중복 리뷰는 작성 불가 안내
  네트워크 오류는 재시도 가능하게 표시
```

이걸 먼저 정리하면 구현이 훨씬 안정적이다.

## Accessibility (접근성)

접근성은 나중에 붙이는 장식이 아니다.

리뷰 폼에서 바로 봐야 하는 것:

```text
textarea에 label이 있는가?
평점 선택은 키보드로 가능한가?
submit 오류가 화면 낭독기에 전달되는가?
모달이라면 열릴 때 focus가 들어가는가?
모달을 닫으면 원래 버튼으로 focus가 돌아가는가?
버튼 disabled 이유를 사용자가 이해할 수 있는가?
에러 문구와 입력 필드가 연결되어 있는가?
```

AI는 접근성을 자주 놓친다. 그래서 프론트 스킬에 명시적으로 넣어둔다.

## Responsive Behavior (반응형 동작)

반응형은 "화면이 줄어들면 알아서 되겠지"가 아니다.

리뷰 폼에서 확인할 것:

```text
모바일에서 평점 버튼이 너무 작지 않은가?
textarea가 화면 밖으로 밀리지 않는가?
submit 버튼 텍스트가 잘리지 않는가?
에러 문구가 입력 필드를 밀어내며 레이아웃을 깨지 않는가?
긴 사용자 이름이나 긴 리뷰 내용이 목록을 깨지 않는가?
```

프론트 구현 후에는 가능하면 실제 브라우저에서 모바일/데스크톱을 확인해야 한다.

## 백엔드로 되돌아가야 하는 순간

프론트 작업 중에 백엔드 질문이 새로 보이면 그냥 추측하지 않는다.

예시:

```text
중복 리뷰 오류 코드가 무엇인지 모른다.
리뷰 작성 가능 여부를 미리 조회하는 API가 없다.
validation error가 어떤 형태로 오는지 모른다.
리뷰 작성 성공 후 알림 상태를 즉시 보여줘야 하는지 모른다.
페이지네이션 방식이 정해지지 않았다.
```

이런 경우에는 프론트 문서에 질문을 남기고, 필요하면 백엔드 스킬로 되돌아간다.

```text
API 계약 누락 -> generate-feature-context 또는 backend-ddd
도메인 규칙 누락 -> backend-ddd
이벤트/비동기 반영 시점 누락 -> backend-eda
테스트 기준 누락 -> testing-tdd
```

## ai-dj에서 쓰는 방식

프론트 기능의 기본 흐름은 다음과 같다.

```text
context-router
-> generate-feature-context
-> testing-tdd
-> frontend-feature
-> frontend-verification
-> verify-context-integrity
```

프론트 스킬은 `feature-context`에서 나온 사용자 목표, UI 영향, API 계약, 상태 규칙을 바탕으로 화면 구현 계획을 만든다.

`frontend-verification`은 이 구현 계획이나 실제 구현 결과를 브라우저 기준으로 다시 확인한다.

중간에 백엔드 계약이 비어 있으면 그 사실을 `Backend follow-ups`로 남기고, 필요한 백엔드 스킬로 되돌아간다.

## 기억할 문장

프론트엔드 기능은 "컴포넌트를 그리는 일"이 아니라, 사용자가 목표를 완료할 수 있도록 모든 상태와 회복 경로를 설계하는 일이다.