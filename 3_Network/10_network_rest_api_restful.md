## REST란?

- REpresentational State Transfer
- 자원(Resource)을 URI로 표현하고, 상태(State)의 전이(Transfer)를 HTTP로 수행
- 로이 필딩(Roy Fielding)이 2000년 논문에서 정의한 아키텍처 스타일
- 웹 기반의 분산 시스템을 위한 아키텍처 패턴

### 핵심 개념

- 자원(Resource): URI(예: /users/1)로 표현되는 대상 (예: 사용자, 게시글 등)
- 행위(Method): HTTP 메서드로 표현됨
    - GET: 자원 조회
    - POST: 자원 생성
    - PUT: 자원 전체 수정
    - PATCH: 자원 일부 수정
    - DELETE: 자원 삭제
- 표현(Representation): 자원의 상태를 전달하는 방식 (보통 JSON)

---

## REST API란?

- REST 원칙을 적용하여 만든 API
- REST 아키텍처를 따르는 웹 서비스의 인터페이스
- HTTP + URI + 메서드 + JSON/XML (Representation)
- 자원에 대한 명확한 주소체계 + 메서드로 액션 정의

예시:

GET /products/1 → ID가 1인 상품 조회

DELETE /users/2 → ID가 2인 사용자 삭제

RESTful이란?

- REST 아키텍처 스타일을 엄격하게 준수한 상태
- API 설계가 REST를 일관되게 반영하고 있을 때 ‘RESTful’하다고 함

즉, RESTful API는 단순한 REST API보다 더 규칙적이고 일관된 API

---

## RESTful API 설계 규칙

1. 자원을 URI로 표현

- URI는 동사(X), 명사(O)
- 예: /getUser (X) → /users (O)

2. HTTP 메서드 활용

- 자원에 대한 행위를 URI가 아닌 HTTP 메서드로 표현

3. 무상태성 (Stateless)

- 서버는 클라이언트의 이전 상태 정보를 저장하지 않음

4. 계층 구조 지원

- 클라이언트는 중간 서버와 직접 통신하지 않음 (캐시, 프록시 허용)

5. 캐시 처리 가능

- HTTP 응답은 캐시 가능해야 함

6. 일관된 인터페이스

- URI, 메서드, 응답 구조 등 API 전반의 일관성

REST API URI 설계 예시

| **작업** | **URI** | **HTTP 메서드** |
| --- | --- | --- |
| 사용자 목록 조회 | /users | GET |
| 사용자 생성 | /users | POST |
| 특정 사용자 조회 | /users/1 | GET |
| 사용자 정보 수정 | /users/1 | PUT |
| 사용자 삭제 | /users/1 | DELETE |

RESTful 하지 않은 API 예시

| **URI** | **문제점** |
| --- | --- |
| /getUserInfo?id=1 | 동사 사용 → REST 원칙 위반 |
| /createNewPost | 동사 사용, HTTP 메서드 무시 |
| /api.php?action=delete&id=3 | REST 원칙 위반, 상태 전달 불명확 |

→ 해결 방법: HTTP 메서드와 URI를 역할에 맞게 분리해야 RESTful

---

## RESTful vs REST API

| **항목** | **REST API** | **RESTful API** |
| --- | --- | --- |
| 정의 | REST 기반의 API | REST 원칙을 충실히 지킨 API |
| URI 구성 | 대체로 규칙적이지 않을 수 있음 | 자원 중심, 명확한 URI 구성 |
| 일관성 | 약함 | 강함 |
| REST 원칙 준수 | 부분적으로 지킬 수 있음 | 원칙을 철저히 지킴  |

---

## REST의 장단점

장점

- URI만 보고도 기능 추측 가능
- HTTP 기반으로 표준화됨
    - HTTP의 표준 메서드(GET, POST, PUT, DELETE 등)를 그대로 활용 별도의 규칙 없이도 API 사용 방식이 통일
- 유지보수성과 확장성 우수
    - REST는 각 URI가 독립적이므로, 기능을 모듈 단위로 추가/수정/삭제하기 쉬움.
    - 무상태(stateless) 기반이기 때문에 서버 확장(스케일아웃)도 쉬움
- 클라이언트-서버 분리로 개발 용이

단점

- 트랜잭션 처리 어려움
    - 예: 주문 생성 → 재고 차감 → 포인트 차감 같은 여러 단계의 작업을 하나의 트랜잭션으로 묶기 어려움.
    - REST는 각 요청이 독립적이기 때문에 작업 중 일부 실패 시 롤백 처리가 어려움
- 무상태성으로 인해 인증 등 부가작업 필요
    - REST는 서버가 클라이언트의 상태를 저장하지 않음 (Stateless).
    - 요청마다 인증 토큰(JWT, 세션 등)을 매번 포함해야 하고, 보안/인증처리를 별도 구현

---

## 퀴즈

무상태(Stateless)의 장점과 단점은?

---

## 출처

- [MDN Web Docs - REST](https://developer.mozilla.org/ko/docs/Glossary/REST)
- https://velog.io/@somfist/REST%EB%9E%80-REST-API-RESTful-API%EC%B0%A8%EC%9D%B4%EC%A0%90