# Backend TDD-DDD-EDA Flow (백엔드 TDD-DDD-EDA 흐름)

English name: Backend TDD-DDD-EDA Flow
한국어 이름: 백엔드 TDD-DDD-EDA 흐름
쉽게 말하면: 백엔드 기능을 테스트, 도메인 책임, 이벤트 전달 순서로 점점 선명하게 만드는 설계 흐름

## 이 문서의 목표

이 문서는 TDD, DDD, EDA를 용어로 외우기 위한 문서가 아니다.

하나의 백엔드 기능을 예로 들어서, 처음에는 막연했던 요구사항이 어떻게 테스트가 되고, 도메인 모델이 되고, 이벤트 흐름이 되는지 따라가 보는 문서다.

읽을 때는 다음 질문 하나만 붙잡으면 된다.

```text
"이 기능이 망가지지 않으려면, 무엇을 먼저 분명히 해야 하지?"
```

TDD, DDD, EDA는 각각 이 질문에 다른 각도로 답한다.

| 관점 | 핵심 질문 | 결과물 |
| --- | --- | --- |
| TDD | 무엇이 맞는 행동이고, 무엇이 틀린 행동인가? | 실패하는 테스트와 검증 기준 |
| DDD | 그 행동과 규칙은 어느 도메인 모델이 책임지는가? | 애그리거트, 불변식, 유스케이스 경계 |
| EDA | 일이 발생한 뒤 다른 기능에는 어떻게 안전하게 알릴 것인가? | 이벤트, 생산자, 소비자, 멱등성, 재시도 |

## 예시 요구사항

아트모아 같은 서비스를 떠올려보자. 전시 상세 페이지에서 사용자가 리뷰를 작성할 수 있다.

요구사항은 이렇게 들어왔다.

```text
사용자는 전시에 리뷰를 작성할 수 있다.
리뷰에는 평점과 내용이 있다.
평점은 1점부터 5점까지 가능하다.
한 사용자는 같은 전시에 리뷰를 한 번만 작성할 수 있다.
리뷰 작성자는 자기 리뷰만 수정할 수 있다.
리뷰가 작성되면 해당 전시를 관심 등록한 사용자에게 알림을 보낸다.
알림 발송이 실패해도 리뷰 작성은 성공해야 한다.
```

겉으로 보면 "리뷰 작성 API 하나 만들면 되겠네"처럼 보인다. 하지만 안에는 여러 종류의 문제가 섞여 있다.

```text
리뷰를 저장하는 문제
중복 리뷰를 막는 문제
평점 범위를 검증하는 문제
작성자 권한을 확인하는 문제
알림을 보내는 문제
알림 실패가 리뷰 작성에 영향을 주지 않게 하는 문제
같은 알림이 두 번 생성되지 않게 하는 문제
```

여기서 바로 코드를 쓰면, AI도 사람도 대개 너무 빨리 구현으로 뛰어든다.

## 먼저 일부러 나쁜 설계를 보자

초보자나 AI가 급하게 만들면 이런 식으로 흐르기 쉽다.

```text
POST /reviews
  1. request body를 받는다.
  2. reviews 테이블에 insert한다.
  3. notificationService.send(...)를 바로 호출한다.
  4. 성공 응답을 반환한다.
```

겉으로는 작동할 수 있다. 하지만 질문을 조금만 던져보면 위험이 보인다.

```text
같은 사용자가 같은 전시에 두 번 리뷰를 쓰면?
평점이 6점이면?
알림 발송이 실패하면 리뷰 작성도 실패해야 하나?
리뷰 저장은 성공했는데 알림 호출만 실패하면?
알림 호출은 성공했는데 API 응답 전에 서버가 죽으면?
notificationService가 같은 요청을 두 번 받으면?
나중에 리뷰 도메인 규칙이 늘어나면 if문은 어디에 쌓이나?
```

이 문제를 막으려고 TDD, DDD, EDA를 한 번에 다 외우려 하면 어렵다. 대신 순서를 나눠서 보면 훨씬 편하다.

```text
TDD: 어떤 행동을 반드시 지켜야 하는지 먼저 고정한다.
DDD: 그 행동을 책임질 도메인 경계를 정한다.
EDA: 리뷰 작성 뒤 알림 같은 후속 처리를 안전하게 분리한다.
```

## 1단계: TDD로 "맞는 행동"을 먼저 고정한다

TDD는 테스트 파일을 먼저 만든다는 뜻으로만 이해하면 반쪽이다.

더 정확히는, 구현 전에 "이 기능이 만족해야 하는 행동"을 실행 가능한 형태로 고정하는 것이다.

위 요구사항에서 테스트로 바꿔야 할 행동은 다음처럼 나눌 수 있다.

| 행동 | 왜 중요한가 | 테스트 레벨 |
| --- | --- | --- |
| 평점은 1점부터 5점까지만 가능하다 | 잘못된 도메인 데이터가 저장되면 안 된다 | Domain test |
| 같은 사용자는 같은 전시에 리뷰를 한 번만 쓴다 | 핵심 비즈니스 규칙이다 | Domain/Application test |
| 작성자만 자기 리뷰를 수정한다 | 권한 규칙이다 | Domain/Application test |
| 리뷰 생성 성공 후 ReviewCreated가 기록된다 | 후속 알림의 출발점이다 | Integration/Application test |
| 리뷰 생성 실패 시 ReviewCreated가 기록되지 않는다 | 실패한 일을 이벤트로 알리면 안 된다 | Integration/Application test |
| 같은 ReviewCreated를 두 번 소비해도 알림은 하나만 생긴다 | 이벤트 중복 전달에 대비해야 한다 | Consumer test |

여기서 핵심은 "전부 E2E로 검증하자"가 아니다. 행동마다 가장 작은 테스트 레벨을 고른다.

### TDD 테스트를 말로 먼저 써보기

처음에는 코드보다 말로 쓰는 게 좋다.

```text
테스트 1: 평점 범위 검증

given:
  사용자가 리뷰를 작성하려고 한다

when:
  평점이 6점이다

then:
  리뷰 생성은 실패한다
  저장소에는 아무 리뷰도 저장되지 않는다
```

```text
테스트 2: 중복 리뷰 방지

given:
  사용자 A가 전시 X에 이미 리뷰를 작성했다

when:
  사용자 A가 전시 X에 다시 리뷰를 작성한다

then:
  DuplicateReview 오류가 발생한다
  새 리뷰는 저장되지 않는다
```

```text
테스트 3: 성공한 리뷰 작성만 이벤트를 남긴다

given:
  사용자 A가 전시 X에 아직 리뷰를 작성하지 않았다

when:
  사용자 A가 정상 리뷰를 작성한다

then:
  리뷰가 저장된다
  ReviewCreated 이벤트가 outbox에 기록된다
```

```text
테스트 4: 실패한 리뷰 작성은 이벤트를 남기지 않는다

given:
  사용자 A가 전시 X에 이미 리뷰를 작성했다

when:
  사용자 A가 전시 X에 다시 리뷰를 작성한다

then:
  DuplicateReview 오류가 발생한다
  ReviewCreated 이벤트는 기록되지 않는다
```

이 정도만 해도 구현 방향이 확 달라진다. 이제 "리뷰 저장 API"가 아니라 "중복, 평점, 이벤트 기록까지 지켜야 하는 유스케이스"가 보인다.

### Red-Green-Refactor를 이 예시에 적용하면

Red:

```text
중복 리뷰 테스트를 먼저 작성한다.
아직 중복 확인 코드가 없으므로 테스트가 실패한다.
```

Green:

```text
리뷰 저장 전에 같은 userId + exhibitionId 리뷰가 있는지 확인한다.
있으면 DuplicateReview 오류를 반환한다.
```

Refactor:

```text
중복 확인 책임이 Application Service에 있는지,
Repository 메서드 이름이 도메인 언어에 맞는지,
DB unique constraint도 필요한지 정리한다.
```

TDD가 좋은 이유는 "테스트가 있으니까 안전하다"가 아니다. 구현 전에 생각해야 할 질문을 앞으로 끌고 오기 때문이다.

## 2단계: DDD로 "책임지는 모델"을 정한다

TDD로 행동을 고정했으면 다음 질문이 나온다.

```text
이 규칙들은 코드 어디에서 책임져야 하지?
```

이게 DDD의 자리다.

DDD는 어려운 설계 용어 모음이 아니라, 비즈니스 규칙을 코드 안에서 길 잃지 않게 만드는 방식이다.

### 먼저 도메인 언어를 뽑는다

요구사항에서 중요한 말을 뽑아보자.

```text
Review
Rating
ReviewAuthor
Exhibition
DuplicateReview
ReviewCreated
InterestedUser
Notification
```

이 말들이 코드, 테스트, 문서에서 같은 뜻으로 쓰이면 이해하기 쉬워진다. 이것을 Ubiquitous Language, 즉 보편 언어라고 부른다.

나쁜 이름은 규칙을 숨긴다.

```text
ContentData
TargetItem
UserAction
StatusCode
```

좋은 이름은 규칙을 드러낸다.

```text
Review
Rating
DuplicateReview
ReviewAuthor
```

### Bounded Context를 나눈다

모든 것을 하나의 "서비스"로 보면 경계가 흐려진다.

이 예시에서는 최소한 이런 경계를 생각할 수 있다.

```text
Reviews Context:
  리뷰 작성, 수정, 삭제, 평점, 중복 작성 방지

Exhibitions Context:
  전시 정보, 전시 공개 상태, 관심 등록

Notifications Context:
  알림 생성, 발송, 실패 재시도
```

여기서 중요한 점은, 리뷰 작성 규칙과 알림 발송 규칙을 같은 곳에 섞지 않는 것이다.

리뷰 도메인의 관심사:

```text
리뷰가 유효한가?
중복 리뷰인가?
작성자가 맞는가?
리뷰 작성이 성공했는가?
```

알림 도메인의 관심사:

```text
누구에게 알림을 보낼 것인가?
이미 만든 알림인가?
발송 실패를 어떻게 재시도할 것인가?
```

### Entity와 Value Object를 구분한다

Review는 식별자가 중요하다. 시간이 지나도 같은 리뷰인지 추적해야 한다.

```text
Review
  id
  authorId
  exhibitionId
  rating
  content
  status
```

이런 것은 Entity에 가깝다.

Rating은 식별자가 중요하지 않다. 5점이라는 값 자체가 중요하다.

```text
Rating
  value: 1..5
```

이런 것은 Value Object에 가깝다.

Rating을 단순 number로 둬도 작동은 한다. 하지만 `Rating` 값 객체로 만들면 "1부터 5까지만 가능하다"는 규칙을 한 곳에 가둘 수 있다.

```text
Rating.create(6)
-> InvalidRating 오류
```

이렇게 되면 평점 검증 if문이 컨트롤러, 서비스, 저장 로직에 흩어지는 것을 막을 수 있다.

### Aggregate를 고른다

애그리거트는 "관련 객체 묶음"이 아니다. 일관성을 지키는 경계다.

이 예시에서는 `Review`를 애그리거트 루트 후보로 볼 수 있다.

```text
Review
  - rating은 1..5여야 한다
  - content는 비어 있으면 안 된다
  - author만 수정할 수 있다
  - deleted 상태에서는 수정할 수 없다
```

하지만 "한 사용자는 같은 전시에 리뷰를 한 번만 작성할 수 있다"는 규칙은 조금 다르다.

이미 존재하는 다른 리뷰를 조회해야 알 수 있기 때문이다.

그래서 이 규칙은 다음 조합으로 지키는 편이 현실적이다.

```text
Application Service:
  같은 userId + exhibitionId 리뷰가 있는지 확인한다.

Repository:
  existsByAuthorAndExhibition(authorId, exhibitionId)를 제공한다.

Database:
  가능하면 unique constraint로 마지막 방어선을 둔다.
```

DDD는 모든 규칙을 무조건 객체 하나에 넣으라는 뜻이 아니다. 규칙이 깨지지 않도록 적절한 경계를 정하라는 뜻이다.

### Application Service는 무엇을 하나

Application Service는 도메인 규칙 그 자체라기보다, 유스케이스를 조율한다.

```text
CreateReviewUseCase
  1. 요청 사용자가 로그인했는지 확인한다.
  2. 전시가 리뷰 가능한 상태인지 확인한다.
  3. 기존 리뷰가 있는지 확인한다.
  4. Review.create(...)를 호출한다.
  5. ReviewRepository.save(review)를 호출한다.
  6. ReviewCreated 이벤트를 outbox에 기록한다.
```

여기서 평점 범위 같은 규칙은 `Review`나 `Rating`이 지킨다.

중복 리뷰처럼 조회가 필요한 규칙은 UseCase와 Repository가 협력한다.

트랜잭션은 보통 Application Service 경계에서 관리한다.

### DDD 결과물을 한 번에 쓰면

```text
Bounded Context:
  Reviews

Ubiquitous Language:
  Review, Rating, ReviewAuthor, DuplicateReview, ReviewCreated

Aggregate Root:
  Review

Value Object:
  Rating

Invariants:
  rating은 1..5
  content는 비어 있으면 안 됨
  작성자만 수정 가능
  같은 authorId + exhibitionId 조합은 하나만 허용

Repository:
  ReviewRepository
    existsByAuthorAndExhibition(authorId, exhibitionId)
    save(review)

Application Service:
  CreateReviewUseCase
```

이제 코드가 단순히 테이블에 데이터를 넣는 수준을 넘어, 비즈니스 규칙을 중심으로 보이기 시작한다.

## 3단계: EDA로 "후속 처리"를 안전하게 분리한다

리뷰 작성 후 알림을 보내야 한다.

여기서 바로 `notificationService.send()`를 호출하면 간단해 보인다.

하지만 요구사항에는 중요한 문장이 있었다.

```text
알림 발송이 실패해도 리뷰 작성은 성공해야 한다.
```

이 말은 리뷰 작성과 알림 발송을 강하게 묶으면 안 된다는 뜻이다.

### 동기 호출로 만들면 생기는 문제

```text
CreateReviewUseCase
  -> ReviewRepository.save(review)
  -> NotificationService.send(review)
```

문제 상황:

```text
리뷰 저장 성공
알림 서비스 장애
API는 실패 응답
사용자는 다시 시도
중복 리뷰 오류 발생
사용자 입장에서는 혼란
```

또는:

```text
리뷰 저장 성공
알림 서비스 호출 성공
API 응답 전에 서버 종료
사용자는 실패로 오해하고 재시도
```

리뷰 작성과 알림 발송은 같은 트랜잭션으로 묶기에 성격이 다르다.

### 이벤트로 표현하면

리뷰가 작성되었다는 사실을 이벤트로 남긴다.

```text
ReviewCreated
```

이 이름은 명령이 아니다.

```text
CreateReview: 리뷰를 만들어라
ReviewCreated: 리뷰가 만들어졌다
```

EDA에서 이벤트 이름은 이미 일어난 사실이어야 한다.

### Domain Event와 Integration Event

내부 도메인에서 "리뷰가 생성되었다"는 사실은 Domain Event가 될 수 있다.

```text
ReviewCreated domain event
```

다른 컨텍스트나 외부 시스템이 구독하는 계약은 Integration Event로 따로 볼 수 있다.

```text
ReviewCreated.v1 integration event
```

처음에는 둘을 하나처럼 구현할 수도 있다. 하지만 개념적으로는 구분하는 게 좋다.

내부 도메인 모델은 바뀔 수 있지만, 외부 이벤트 계약은 소비자가 있으므로 함부로 바꾸기 어렵기 때문이다.

### Outbox Pattern이 왜 필요한가

리뷰 저장과 이벤트 발행 사이에는 위험한 틈이 있다.

나쁜 흐름 1:

```text
1. 리뷰 저장 성공
2. 이벤트 발행 실패
3. 리뷰는 있는데 알림은 절대 가지 않음
```

나쁜 흐름 2:

```text
1. 이벤트 발행 성공
2. 리뷰 저장 실패
3. 소비자는 존재하지 않는 리뷰를 보고 알림을 만들려고 함
```

Outbox Pattern은 이 틈을 줄인다.

```text
한 트랜잭션 안에서:
  1. Review 저장
  2. Outbox에 ReviewCreated 기록
  3. commit

별도 프로세스:
  4. Outbox에서 발행 안 된 이벤트 조회
  5. 메시지 브로커로 발행
  6. 발행 완료 표시
```

이렇게 하면 리뷰 저장과 이벤트 기록이 함께 성공하거나 함께 실패한다.

### Consumer는 멱등해야 한다

메시지는 두 번 올 수 있다고 생각하는 편이 안전하다.

예를 들어 알림 consumer가 이렇게 동작한다고 하자.

```text
ReviewCreated(eventId=abc) 수신
-> 관심 등록 사용자 조회
-> 알림 생성
-> 처리 완료 ack
```

그런데 알림 생성은 성공했지만 ack 전에 consumer가 죽으면, 메시지 브로커는 같은 이벤트를 다시 줄 수 있다.

멱등성이 없으면:

```text
첫 번째 처리: 알림 생성
두 번째 처리: 알림 또 생성
결과: 같은 알림이 두 번 감
```

멱등성이 있으면:

```text
첫 번째 처리:
  eventId=abc 처리 기록이 없음
  알림 생성
  eventId=abc 처리 기록 저장

두 번째 처리:
  eventId=abc 처리 기록이 있음
  아무 부작용 없이 성공 처리
```

멱등성은 "중복이 안 오게 한다"가 아니라 "중복이 와도 망가지지 않게 한다"이다.

### Retry와 Dead Letter Queue

모든 실패를 같은 방식으로 처리하면 안 된다.

```text
일시적 실패:
  알림 서버가 잠깐 느림
  네트워크가 잠깐 끊김
  -> 재시도할 가치가 있음

영구적 실패:
  payload에 필수 필드가 없음
  consumer 코드가 처리할 수 없는 버전
  -> 계속 재시도해도 해결 안 됨
```

그래서 재시도와 DLQ가 필요하다.

```text
Retry:
  일시적 실패는 몇 번 다시 시도한다.

Dead Letter Queue:
  계속 실패하거나 처리 불가능한 메시지를 따로 보관한다.

Reprocessing:
  문제를 고친 뒤 DLQ 메시지를 다시 처리한다.
```

EDA 설계에서 실패 처리는 부록이 아니다. 핵심 설계다.

## 전체 흐름을 한 번에 보면

```text
사용자
-> POST /reviews
-> CreateReviewUseCase
   -> 중복 리뷰 확인
   -> Rating 검증
   -> Review 생성
   -> Review 저장
   -> Outbox에 ReviewCreated 기록
-> API 성공 응답

나중에:
OutboxPublisher
-> ReviewCreated 발행
-> NotificationConsumer
   -> eventId 멱등성 확인
   -> 관심 등록 사용자 조회
   -> 알림 생성
   -> 처리 기록 저장
```

이 흐름에서 TDD, DDD, EDA는 각각 이렇게 자리 잡는다.

```text
TDD:
  중복 리뷰는 실패해야 한다.
  평점 6점은 실패해야 한다.
  성공한 리뷰 작성만 이벤트를 남겨야 한다.
  같은 이벤트를 두 번 소비해도 알림은 하나여야 한다.

DDD:
  Review, Rating, DuplicateReview 같은 도메인 언어를 정한다.
  Review가 어떤 규칙을 직접 책임질지 정한다.
  CreateReviewUseCase가 어떤 흐름을 조율할지 정한다.
  ReviewRepository가 어떤 조회와 저장을 제공할지 정한다.

EDA:
  ReviewCreated를 어떤 payload로 남길지 정한다.
  Outbox로 이벤트 유실을 줄인다.
  NotificationConsumer를 멱등하게 만든다.
  Retry와 DLQ로 실패를 운영 가능하게 만든다.
```

## 왜 이 순서가 좋은가

TDD 없이 DDD부터 하면 모델이 멋있어 보여도 실제 요구사항을 놓칠 수 있다.

```text
Review 클래스를 만들었다.
Rating 값 객체도 만들었다.
그런데 중복 리뷰 실패 케이스 테스트가 없다.
결과: 중요한 규칙이 깨져도 모른다.
```

DDD 없이 EDA부터 하면 이벤트는 있는데 어디서 발생해야 하는지 흐려진다.

```text
ReviewCreated 이벤트를 만들었다.
그런데 리뷰 생성의 성공 기준이 불명확하다.
결과: 실패한 리뷰에도 이벤트가 나갈 수 있다.
```

EDA 없이 동기 호출만 하면 후속 처리 실패가 핵심 기능을 흔들 수 있다.

```text
리뷰 저장은 성공했다.
알림 발송이 실패했다.
결과: 리뷰 작성 API까지 실패처럼 보인다.
```

그래서 기본 순서는 이렇게 둔다.

```text
TDD -> DDD -> EDA
```

정확히는 한 번만 도는 직선이 아니라, 필요할 때 되돌아가는 반복이다.

```text
TDD로 행동을 잡는다
-> DDD로 책임을 나눈다
-> 새 규칙이 보이면 TDD로 돌아가 테스트를 추가한다
-> EDA로 후속 처리를 설계한다
-> 실패/중복 케이스가 보이면 TDD로 돌아가 테스트를 추가한다
```

## AI에게 시킬 때 좋은 요청 예시

나쁜 요청:

```text
리뷰 기능 만들어줘.
```

이렇게 말하면 AI는 바로 코드로 뛰어들 수 있다.

더 좋은 요청:

```text
리뷰 작성 기능을 바로 구현하지 말고,
먼저 TDD 관점에서 성공/실패 테스트 시나리오를 정리해줘.
그 다음 DDD 관점에서 Review, Rating, DuplicateReview, Repository, UseCase 책임을 나눠줘.
마지막으로 리뷰 작성 후 알림을 EDA로 분리할지 판단하고,
필요하면 ReviewCreated 이벤트, outbox, consumer 멱등성, retry/DLQ까지 설계해줘.
각 단계에서 근거가 부족한 것은 open question으로 남겨줘.
```

ai-dj에서는 이 요청을 다음 스킬 체인으로 처리하게 만든다.

```text
context-router
-> generate-feature-context
-> testing-tdd
-> backend-ddd
-> backend-eda
-> verify-context-integrity
```

## 스스로 점검하는 체크리스트

TDD 체크:

```text
성공 케이스가 있는가?
실패 케이스가 있는가?
권한 케이스가 있는가?
상태 변화 케이스가 있는가?
이벤트가 나가야 하는 조건과 나가면 안 되는 조건이 있는가?
```

DDD 체크:

```text
도메인 언어가 코드 이름에 반영되었는가?
불변식이 어디에서 지켜지는지 보이는가?
Application Service와 Domain Model의 책임이 섞이지 않았는가?
Repository가 도메인 관점의 저장 경계로 보이는가?
DB 테이블 모양이 그대로 도메인 모델이 되지는 않았는가?
```

EDA 체크:

```text
이벤트 이름이 이미 일어난 사실인가?
이벤트는 성공한 상태 변화 뒤에만 기록되는가?
payload가 필요한 정보만 담고 있는가?
consumer는 같은 이벤트를 두 번 받아도 안전한가?
재시도와 DLQ, 재처리 방법이 있는가?
```

## 마지막으로 기억할 문장

TDD는 "기능이 맞는지"를 먼저 묻는다.

DDD는 "그 기능의 규칙을 누가 책임지는지"를 묻는다.

EDA는 "그 기능이 끝난 뒤 다른 일로 어떻게 안전하게 이어지는지"를 묻는다.

이 세 질문이 동시에 보이면, 백엔드 설계가 단순 CRUD에서 한 단계 올라간다.
