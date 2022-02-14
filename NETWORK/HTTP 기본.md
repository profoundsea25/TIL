> 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의 요약입니다.

# HTTP 기본
## 모든 것이 HTTP(HyperText Transfer Protocol)
- HTTP 메시지에 모든 것을 전송
  - HTML, TEXT, IMAGE, 음성, 영상, 파일, JSON, XML(API)
- 거의 모든 형태의 데이터 전송 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용
- HTTP/1.1을 가장 많이 사용, 우리에게 가장 중요한 버전
- HTTP/2, HTTP/3 은 성능 개선에 초점

#### 기반 프로토콜
- TCP : HTTP/1.1(메인), HTTP/2
- UDP : HTTP/3

#### HTTP 특징
- 클라이언트 서버 구조
- 무상태 프로토콜(Stateless), 비연결성
- HTTP 메시지
- 단순함, 확장 가능

## 클라이언트 서버 구조
- Request - Response 구조
- 클라이언트는 서버에 요청을 보내고, 응답을 대기
- 서버가 요청에 대한 결과를 만들어서 응답
- 클라이언트(UI, 사용성)와 서버(비즈니스 로직, 데이터)를 개념적으로 분리하는 것이 중요

#### 무상태(Stateless) 프로토콜
- 서버가 클라이언트의 상태를 보존하지 않음
- 장점 : 서버 확장성이 높음(스케일 아웃, 수평 확장)
- 단점 : 클라이언트가 추가 데이터 전송

#### Stateful(상태 유지)란?
- 서버가 클라이언트의 이전 상태(문맥)을 보존

#### Stateful, Stateless 차이
- 상태 유지 : 중간에 다른 점원으로 바뀌면 안 된다. (중간에 다른 점원으로 바뀔 때 상태 정보를 다른 점원에게 미리 알려줘야 한다.)
  - 즉 항상 같은 서버가 유지되어야 한다.
- 무상태 : 중간에 다른 점원으로 바뀌어도 된다.
  - 갑자기 고객이 증가해도 점원을 대거 투입할 수 있다.
  - 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다.
  - 즉 아무 서버나 호출해도 된다.
- 무상태는 응답 서버를 쉽게 바꿀 수 있다. -> 무한한 서버 증설 가능

#### Stateless 실무 한계
- 모든 것을 무상태로 설계할 수 있는 경우도 있고 없는 경우도 있다.
- 무상태
  - 예) 로그인이 필요없는 단순한 서비스 소개 화면
- 상태 유지
  - 예) 로그인
- 로그인한 사용자의 경우 로그인 헀다는 상태를 서버에 유지
- 일반적으로 브라우저 쿠키와 서버 세션 등을 사용해서 상태 유지
- 상태 유지는 최소한만 사용
- + 단점 : Stateful보다 데이터를 너무 많이 보낸다.

## 비연결성(Connectionless)
- 연결을 유지하는 모델의 경우, 서버에서 연결을 계속 유지하기 위해 서버 자원을 소모하게 된다.
- 연결을 유지하지 않는 모델의 경우, 서버는 연결 유지를 할 필요가 없기 때문에, 최소한의 자원을 사용하게 된다.
- HTTP는 기본이 연결을 유지하지 않는 모델
- 일반적으로 초 단위 이하의 빠른 속도로 응답
- 서버 자원을 매우 효율적으로 사용할 수 있음

#### 비연결성의 한계와 극복
- TCP/IP 연결을 새로 맺어야 함. 3 way handshake 시간 추가
- 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, CSS, 추가 이미지 등 수 많은 자원이 함께 다운로드
- 지금은 HTTP 지속 연결(Persistence Connections)로 문제 해결
- HTTP/2, HTTP/3 에서 더 많은 최적화

#### Stateless를 기억하자
- 서버 개발자들이 어려워하는 업무 = 정말 같은 시간에 딱 맞추어 발생하는 대용량 트래픽 (ex 티케팅)
- 어떻게든 Stateless하게 설계해야 한다.
- 최대한 무상태로 설계해라. 어쩔 수 없는 부분만 Stateful하게 설계해라.

## HTTP 메시지
#### HTTP 메시지 구조
- start-line 시작 라인
- header 헤더
- empty line 공백 라인 (CRLF) : 반드시 있어야 함
- message body

### 시작 라인
#### 요청 메시지
```
GET /search?q=hello&hl=ko HTTP/1.1
Host: www.google.com
```
- start-line = request-line
- request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)
  - 즉 `HTTP메서드 요청대상 HTTP버전`
  - request-target = absolute-path[?query] (절대경로[?쿼리])
    - 절대경로 = "/"로 시작하는 경로

#### 응답 메시지
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 3423

//메시지 내용
```
- start-line = status-line
- status-line = HTTP-version SP status-code SP reason-phrase CRLF
  - 즉 `HTTP메서드 HTTP상태코드 이유문구`

### HTTP 헤더
- header-field = field-name":" OWS field-value OWS (OWS:띄어쓰기 허용)
- field-name은 대소문자 구분 없음

#### 용도
- HTTP 전송에 필요한 모든 부가정보 (메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보 등)
- 표준 헤더가 너무 많음
- 필요시 임의의 헤더 추가 가능

### HTTP 메시지 바디
- 실제 전송할 데이터가 들어가 있음
- byte로 표현할 수 있는 모든 데이터 전송 가능
