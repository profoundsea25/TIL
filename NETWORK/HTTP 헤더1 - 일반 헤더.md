> 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의 요약입니다.

# HTTP 헤더1 - 일반 헤더
## HTTP 헤더 개요
### HTTP 헤더의 용도
- HTTP 전송에 필요한 모든 부가 정보
- 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등
- 표준 헤더가 너무 많음
- 필요시 임의의 헤더 추가 가능

### HTTP 헤더의 분류 - RFC2616(과거)
- General 헤더 : 메시지 전체에 적용되는 정보
- Request 헤더 : 요청 정보
- Response 헤더 : 응답 정보
- Entity 헤더 : 엔티티 바디 정보

### HTTP BODY- RFC2616(과거)
- 메시지 본문(message body)은 엔티티 본문(entity body)을 전달하는데 사용
- 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
- 엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보 제공
  - 데이터 유형(html, json), 데이터 길이, 압축 정보 등

### HTTP 헤더의 분류 - RFC723x
- 엔티티(Entity) -> 표현(Representation)
- Representation = Representation Metadata + Representation Data
- 표현 = 표현 메타데이터 + 표현 데이터

### HTTP BODY - RFC7230(최신)
- 메시지 본문(message body)을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드(payload)
- 표현은 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
  - 데이터 유형(html, json), 데이터 길이, 압축 정도 등등
- 참고)2 표현 헤더는 표현 메타데이터와, 페이로드 메시지를 구분해야 하지만, 복잡해서 이는 생략


## 표현
- Content-Type : 표현 데이터의 형식
- Content-Encoding : 표현 데이터의 압축 방식
- Content-Language : 표현 데이터의 자연 언어
- Content-Length : 표현 데이터의 길이
- 표현 헤더는 전송, 응답 둘다 사용

### Content-Type
- 표현 데이터의 형식 설명
- 미디어 타입, 문자 인코딩 등
  - text/html; charset=utf-8
  - application/json
  - image/png

### Content-Encoding
- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
  - gzip, deflate, identity(압축 안 함)

### Content-Language
- 표현 데이터의 자연 언어를 표현
  - ko, en, en-US

### Content-Length
- 표현 데이터의 길이
- 바이트 단위
- Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안 됨


## 협상(콘텐츠 네고시에이션)
- 클라이언트가 선호하는 표현 요청
- Accept : 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset : 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
- Accept-Language : 클라이언트가 선호하는 자연 언어
- 협상 헤더는 요청시에만 사용

### 협상과 우선순위 1
- Quality Values(q) 값 사용
- 0~1, 클수록 높은 우선순위
- 생략하면 1
- 예) Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
  - 1. ko-KR;q=1(생략)
  - 2. ko;q=0.9
  - 3. en-US;q=0.8
  - 4. en;q=0.7

### 협상과 우선순위 2
- 구체적인 것이 우선한다.
- 예) Accept: text/\*, text/plain, text/plain;format=flowed, \*/\*
- 1. text/plain;format=flowed
- 2. text/plain
- 3. text/\*
- 4. \*/\*

### 협상과 우선순위 3
- 구체적인 것을 기준으로 미디어 타입을 맞춘다.
- 예) Accept: Accept: text/\*;q=0.3, text/plain;q=0.7, text/html;level=1, text/html;level=2;q=0.4, \*/\*;q=0.5
- text/html;level=1 : 1
- text/html : 0.7
- text/plain : 0.3
- image/jpeg : 0.5
- text/html;level=2 : 0.4
- text/html;level=3 : 0.7


## 전송 방식
- 단순 전송
- 압축 전송
- 분할 전송
- 범위 전송

### 단순 전송
- Content-Length의 길이를 알 수 있을 때 쓴다.
- 한 번에 요청하고 한 번에 받는다.

### 압축 전송
- Content-Encoding을 헤더에 압축 방식과 함께 추가로 넣어줘야 함.

### 분할 전송
- 헤더에 Transfer-Encoding: chunked 등으로 표현
- Content-Length를 표현할 수 없음.

### 범위 전송
- Range, Content-Range
- 범위를 지정해서 요청할 수 있음.


## 일반 정보
- From : 유저 에이전트의 이메일 정보
 - 일반적으로 잘 사용되지 않음
 - 검색 엔진 같은 곳에서 주로 사용
 - 요청에서 사용
- Referer : 이전 웹 페이지 주소
  - 현재 요청된 페이지의 이전 웹 페이지 주소
  - A -> B로 이동하는 경우 B를 요청할 때 Referer: A 를 포함해서 요청
  - Referer를 사용해서 유입 경로 분석 가능
  - 요청에서 사용
  - 참고) referer는 단어 referrer의 오타
- User-Agent : 유저 에이전트 애플리케이션 정보
  - 클라이언트의 애플리케이션 정보(웹 브라우저 정보 등)
  - 통계 정보
  - 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
  - 요청에서 사용
- Server : 요청을 처리하는(표현 데이터를 보내주는 진짜) ORIGIN 서버의 소프트웨어 정보
  - 예) Server: Apache/2.2.22 (Debian)
  - 응답에서 사용
- Date : 메시지가 발생한 날짜와 시간
  - 응답에서 사용


## 특별한 정보
- Host : 요청한 호스트 정보 (도메인)
  - 요청에서 사용
  - 필수
  - 하나의 서버가 여러 도메인을 처리해야 할 때
  - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
- Location : 페이지 리다이렉션
  - 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
  - 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI
  - 3xx (Redirection): Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴
- Allow : 허용 가능한 HTTP 메서드
  - 405 (Method Not Allowed)에서 응답에 포함해야 함
- Retry-After : 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
  - 503 (Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
  - 날짜 혹은 초단위로 표현 가능


## 인증
- Authorization : 클라이언트 인증 정보를 서버에 전달
- WWW-Authenticate : 리소스 접근시 필요한 인증 방법 정의
  - 401 Unauthorized 응답과 함께 사용
  - 인증을 하려면 이 헤더에 쓰여있는 값을 참조해서 헤더를 작성해야 함


## 쿠키
- Set-Cookie : 서버에서 클라이언트로 쿠키 전달(응답)
- Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

### Stateless
- HTTP는 무상태(Stateless) 프로토콜이다.
- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다.
- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.
- 클라이언트와 서버는 서로 상태를 유지하지 않는다.

### 대안? - 모든 요청에 사용자 정보 포함
- 모든 요청과 링크에 사용자 정보를 포함하도록 개발
- 보안 문제, 개발의 어려움 등
- 브라우저를 완전히 종료하고 다시 열었을 때 처리가 어려움

### 쿠키
- 예) set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure
- 로그인 정보가 들어오면 서버에서 응답으로 Set-Cookie라는 헤더를 포함해 전송한다. 클라이언트는 이 헤더를 통해 쿠키 저장소에 쿠키를 저장한다.
- 이후 클라이언트는 모든 요청에 쿠키 정보를 자동으로 포함한다.
- 사용처
  - 사용자 로그인 세션 관리
  - 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송됨
  - 네트워크 트래픽 추가 유발
  - 따라서 최소한의 정보만 사용하는 것이 좋음 (세션 ID, 인증 토큰)
  - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지(localStorage, sessionStorage) 참고
- 주의
  - 보안에 민감한 데이터는 저장하면 안 됨 (주민번호, 신용카드 번호 등)

### 쿠키 - 생명주기
- Set-Cookie: expires=Sat, 26-Dec-2020 00:00:00 GMT
  - 만료일이 되면 쿠키 삭제
- Set-Cookie: max-age=3600 (3600초)
  - 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시까지만 유지
- 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지

### 쿠키 - 도메인
- domain=example.com
- 명시 : 명시한 문서 기준 도메인 + 서브 도메인 포함
  - domain=example.com를 지정해서 쿠키 생성
    - example.com은 물론
    - dev.example.com도 쿠키 접근 가능
- 생략 : 현재 문서 기준 도메인만 적용
  - example.com에서 쿠키를 생성하고 domain 지정을 생략
    - example.com에서만 쿠키 접근
    - dev.example.com는 쿠키 접근 불가

### 쿠키 - 경로
- path=/home
- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
- 일반적으로 path=/ 즉, 루트로 지정

### 쿠키 보안
- Secure
  - 원래 쿠키는 HTTP, HTTPS를 구분하지 않고 전송
  - Secure를 적용하면 HTTPS인 경우에만 전송
- HttpOnly
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가(document.cookie)
  - HTTP 전송에만 사용
- SameSite
  - XSRF 공격 방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송
