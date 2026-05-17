# HTTP 기초

## NET-001 HTTP에 대해 설명해 주세요.

### 1. 답변
HTTP는 웹에서 클라이언트와 서버가 데이터를 주고받기 위한 애플리케이션 계층 프로토콜입니다.  
브라우저와 서버 간 통신의 표준이라고 이해하면 됩니다.

예를 들어 사용자가 웹사이트에 접속하면 브라우저가 서버에 HTTP 요청(Request)을 보내고, 서버는 HTML, JSON, 이미지 같은 데이터를 HTTP 응답(Response)으로 반환합니다.

HTTP의 핵심 특징은 다음과 같습니다.

- 요청/응답 기반 프로토콜
- 텍스트 기반 프로토콜
- TCP 위에서 동작
- Stateless 프로토콜

동작 방식은 간단하게 보면:

1. 클라이언트가 요청을 보냄
2. 서버가 요청을 처리
3. 응답 반환
4. 연결 종료 또는 유지

이런 흐름입니다.

실무에서는 주로 REST API 형태로 많이 사용합니다.  
예를 들어 Spring 서버에서는:

- GET: 조회
- POST: 생성
- PUT/PATCH: 수정
- DELETE: 삭제

처럼 HTTP Method를 통해 행위를 구분합니다.

또 HTTP는 Header와 Body 구조를 가집니다.

예를 들어:

- Header: 인증 토큰, Content-Type, 캐시 정보
- Body: 실제 데이터(JSON 등)

를 담습니다.

추가로 HTTP 버전에 따라 성능 차이도 있습니다.

- HTTP/1.1: keep-alive 지원
- HTTP/2: 멀티플렉싱 지원
- HTTP/3: UDP 기반 QUIC 사용

특히 HTTP/2는 하나의 TCP 연결에서 여러 요청을 동시에 처리할 수 있어서 API 호출이 많은 서비스에서 성능 개선 효과가 큽니다.

실무에서는 단순히 “HTTP를 안다”보다도:

- 무상태성
- 캐시 전략
- Keep-Alive
- 멱등성(Idempotency)
- 인증 방식(JWT, Cookie, Session)

같은 개념까지 연결해서 이해하는 게 중요합니다.

---

### 2. 꼬리 질문

1. HTTP와 HTTPS의 차이점은 무엇인가요?
2. HTTP/1.1과 HTTP/2의 성능 차이를 설명해 주세요.
3. REST API에서 HTTP Method의 멱등성(Idempotency)이 왜 중요한가요?

---

### 3. 심화 학습 포인트

- HTTP Keep-Alive
- HTTP/2 멀티플렉싱
- HTTP/3와 QUIC
- RESTful API 설계
- Cookie vs Session vs JWT
- OAuth 2.0 / OpenID Connect
- 멱등성(Idempotency)과 재시도 전략
- 캐시(Cache-Control, ETag)
- CDN과 HTTP 캐싱 전략

---

## NET-002 HTTP의 특성인 Stateless에 대해 설명해 주세요.

### 1.답변

HTTP의 Stateless는 서버가 클라이언트의 이전 요청 상태를 저장하지 않는다는 의미입니다.

즉, 각각의 요청은 완전히 독립적으로 처리됩니다.

예를 들어 사용자가 로그인 후 상품 조회를 요청하더라도, 서버는 이전 로그인 요청을 기억하지 않습니다.  
그래서 매 요청마다 인증 정보(JWT, Session ID 등)를 함께 보내야 합니다.

이런 Stateless 특성의 장점은 확장성입니다.

서버가 사용자 상태를 메모리에 유지하지 않아도 되기 때문에:

- 서버 증설이 쉬움
- 로드밸런싱에 유리
- 장애 복구가 쉬움
- 수평 확장(scale-out)에 적합

합니다.

특히 MSA나 클라우드 환경에서는 Stateless 구조가 거의 필수에 가깝습니다.

예를 들어 API 서버가 10대로 늘어나더라도 요청이 어느 서버로 가든 동일하게 처리할 수 있습니다.

반면 단점도 있습니다.

매 요청마다 인증 정보나 상태 정보를 보내야 해서:

- 네트워크 비용 증가
- 인증 처리 비용 증가
- 클라이언트 책임 증가

문제가 생길 수 있습니다.

그래서 실제 서비스에서는 완전한 Stateless만 사용하지 않고 보완 전략을 함께 사용합니다.

대표적으로:

- JWT 기반 인증
- Redis 세션 저장소
- Access Token + Refresh Token
- API Gateway 인증 캐시

등을 사용합니다.

예를 들어 Spring Session + Redis를 사용하면 여러 서버가 동일 세션 정보를 공유할 수 있어서 세션 기반 로그인도 scale-out이 가능해집니다.

또 실무에서는 Stateless와 Stateful 차이를 면접에서 자주 연결해서 물어봅니다.

- Stateful: 서버가 상태 기억
- Stateless: 클라이언트가 상태 전달

이라고 이해하면 됩니다.

그리고 중요한 점은:

> "HTTP 자체는 Stateless지만, 서비스는 Stateful하게 동작할 수 있다"

입니다.

예를 들어 장바구니, 로그인 상태 같은 기능은 결국 상태 관리가 필요하기 때문에 Cookie, Session, JWT 등을 이용해 애플리케이션 레벨에서 상태를 구현합니다.

---

### 2. 꼬리 질문

1. JWT 기반 인증이 Stateless에 적합한 이유는 무엇인가요?
2. Stateless 환경에서 로그아웃 기능은 어떻게 구현할 수 있나요?
3. 세션 기반 인증을 사용하는데 서버를 Scale-Out 해야 한다면 어떤 방식으로 해결하시겠습니까?

---

### 3. 심화 학습 포인트

- Cookie vs Session vs JWT
- Redis Session Cluster
- Load Balancer와 Sticky Session
- API Gateway 인증 구조
- OAuth 2.0 / OpenID Connect
- Access Token / Refresh Token 전략
- Stateless 아키텍처와 MSA
- 인증/인가(Authentication vs Authorization)
- JWT 보안 이슈
- 세션 클러스터링
## NET-003 Stateless와 Connectionless에 대해 설명해 주세요.

## NET-004 왜 HTTP는 Stateless 구조를 채택하고 있을까요?

## NET-005 Connectionless의 논리대로면 성능이 되게 좋지 않을 것으로 보이는데, 해결 방법이 있을까요?

## NET-006 TCP의 keep-alive와 HTTP의 keep-alive의 차이는 무엇인가요?