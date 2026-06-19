# Concept Glossary (개념 용어표)

ai-dj를 공부할 때 자주 나오는 영어 개념을 한국어 이름과 함께 정리한다.

영어 이름은 문서, 논문, 도구에서 자주 보이는 표현이다. 한국어 이름은 우리가 이해하고 대화할 때 쓰는 이름이다.

| English name | 한국어 이름 | 쉽게 말하면 | 관련 문서 |
| --- | --- | --- | --- |
| AI Agent Harness | AI 에이전트 하네스 | AI가 안정적으로 일하도록 둘러싼 실행 시스템 | `ai-agent-harness--ai-에이전트-하네스.md` |
| Context Routing | 컨텍스트 라우팅 | 작업 전에 어떤 파일을 읽을지 고르는 과정 | `context-routing--컨텍스트-라우팅.md` |
| Feature Context | 기능 컨텍스트 | 기능 구현 전에 필요한 정보를 작업 가능한 묶음으로 정리한 것 | `feature-context--기능-컨텍스트.md` |
| Context Integrity | 컨텍스트 무결성 | AI 결과가 실제 근거 파일과 맞는지 확인하는 기준 | `context-integrity--컨텍스트-무결성.md` |
| Living Documentation | 살아있는 문서 | 프로젝트 변화와 함께 계속 업데이트되는 문서 | `living-documentation--살아있는-문서.md` |
| Skill | 스킬 | 특정 종류의 작업을 반복 가능하게 만든 절차 | `skills--스킬.md` |
| Supervisor | 작업 지휘자 | 사용자 요청을 스킬 체인으로 바꾸는 상위 진행자 | `supervisor--작업-지휘자.md` |
| Skill Roadmap | 스킬 로드맵 | 어떤 스킬을 어떤 순서로 만들지 정리한 계획 | `skill-roadmap--스킬-로드맵.md` |
| Project Profile | 프로젝트 프로필 | 특정 프로젝트의 파일 맵, 용어, 결정, 규칙 묶음 | `project-profiles/` |
| Skill Chain | 스킬 체인 | 여러 스킬을 순서대로 연결한 작업 흐름 | `supervisor--작업-지휘자.md` |
| Verification | 검증 | 결과가 맞는지 확인하는 단계 | `context-integrity--컨텍스트-무결성.md` |
| Durable Knowledge | 오래 남길 지식 | 다음 작업에서도 써야 하는 결정, 규칙, 학습 내용 | `living-documentation--살아있는-문서.md` |
| Test-Driven Development | 테스트 주도 개발 | 코드를 쓰기 전에 원하는 동작을 테스트로 먼저 고정하는 방식 | `testing-tdd--테스트-주도-개발.md` |
| Backend TDD-DDD-EDA Flow | 백엔드 TDD-DDD-EDA 흐름 | 리뷰 기능 예제로 행동, 도메인 책임, 이벤트 전달을 순서대로 설계하는 튜토리얼 | `backend-tdd-ddd-eda-flow--백엔드-tdd-ddd-eda-흐름.md` |
| Red-Green-Refactor | 레드-그린-리팩터 | 실패 테스트, 통과 구현, 구조 정리를 반복하는 TDD 흐름 | `testing-tdd--테스트-주도-개발.md` |
| Failing Test | 실패하는 테스트 | 아직 구현되지 않은 기대 행동을 먼저 보여주는 테스트 | `testing-tdd--테스트-주도-개발.md` |
| Domain-Driven Design | 도메인 주도 설계 | 비즈니스 규칙과 언어를 코드 구조의 중심에 두는 방식 | `domain-driven-design--도메인-주도-설계.md` |
| Bounded Context | 바운디드 컨텍스트 | 같은 단어가 같은 의미로 쓰이는 업무 경계 | `domain-driven-design--도메인-주도-설계.md` |
| Ubiquitous Language | 보편 언어 | 기획, 개발, 문서, 코드가 함께 쓰는 도메인 언어 | `domain-driven-design--도메인-주도-설계.md` |
| Aggregate | 애그리거트 | 함께 일관성을 지켜야 하는 도메인 객체 묶음 | `domain-driven-design--도메인-주도-설계.md` |
| Aggregate Root | 애그리거트 루트 | 애그리거트 밖에서 접근하는 대표 객체 | `domain-driven-design--도메인-주도-설계.md` |
| Invariant | 불변식 | 어떤 변경 뒤에도 항상 참이어야 하는 규칙 | `domain-driven-design--도메인-주도-설계.md` |
| Repository | 리포지토리 | 애그리거트를 저장소에서 가져오고 저장하는 경계 | `domain-driven-design--도메인-주도-설계.md` |
| Event-Driven Architecture | 이벤트 주도 아키텍처 | 이벤트를 중심으로 시스템을 느슨하게 연결하는 구조 | `event-driven-architecture--이벤트-주도-아키텍처.md` |
| Domain Event | 도메인 이벤트 | 한 도메인 안에서 실제로 발생한 중요한 사실 | `event-driven-architecture--이벤트-주도-아키텍처.md` |
| Integration Event | 통합 이벤트 | 다른 시스템이나 바운디드 컨텍스트에 알리기 위한 이벤트 계약 | `event-driven-architecture--이벤트-주도-아키텍처.md` |
| Producer | 생산자 | 이벤트를 발행하는 쪽 | `event-driven-architecture--이벤트-주도-아키텍처.md` |
| Consumer | 소비자 | 이벤트를 받아 처리하는 쪽 | `event-driven-architecture--이벤트-주도-아키텍처.md` |
| Idempotency | 멱등성 | 같은 메시지를 여러 번 처리해도 결과가 망가지지 않는 성질 | `event-driven-architecture--이벤트-주도-아키텍처.md` |
| Outbox Pattern | 아웃박스 패턴 | 상태 변경과 이벤트 기록을 같은 트랜잭션에 저장한 뒤 나중에 발행하는 방식 | `event-driven-architecture--이벤트-주도-아키텍처.md` |
| Dead Letter Queue | 데드 레터 큐 | 계속 처리할 수 없는 메시지를 따로 보내는 곳 | `event-driven-architecture--이벤트-주도-아키텍처.md` |
| Frontend Feature Development | 프론트엔드 기능 개발 | 사용자 흐름, 화면 상태, 컴포넌트, API 계약을 함께 설계하는 방식 | `frontend-feature-development--프론트엔드-기능-개발.md` |
| User Flow | 사용자 흐름 | 사용자가 목표를 완료하기까지 거치는 단계 | `frontend-feature-development--프론트엔드-기능-개발.md` |
| UI State | 화면 상태 | 로딩, 빈 화면, 오류, 성공, 비활성 등 화면의 상황 | `frontend-feature-development--프론트엔드-기능-개발.md` |
| Server State | 서버 상태 | API나 백엔드에서 가져오고 저장하는 데이터 상태 | `frontend-feature-development--프론트엔드-기능-개발.md` |
| Client UI State | 클라이언트 UI 상태 | 입력 draft, 열림/닫힘, 선택 탭 같은 브라우저 안 상호작용 상태 | `frontend-feature-development--프론트엔드-기능-개발.md` |
| API Contract | API 계약 | 프론트와 백엔드가 합의한 요청, 응답, 오류 형식 | `frontend-feature-development--프론트엔드-기능-개발.md` |
| Accessibility | 접근성 | 키보드, 포커스, 라벨, 의미 구조 등 누구나 쓸 수 있게 하는 기준 | `frontend-feature-development--프론트엔드-기능-개발.md` |

## 작성 규칙

새 개념 문서를 추가할 때는 다음을 함께 적는다.

```text
English name: ...
한국어 이름: ...
쉽게 말하면: ...
```

제목도 가능하면 다음 형식을 쓴다.

```text
# English Name (한국어 이름)
```

파일명도 가능하면 다음 형식을 쓴다.

```text
english-name--한국어-이름.md
```
