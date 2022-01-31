> 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의 요약입니다.

## 인터넷 네트워크
### IP(인터넷 프로토콜)
#### IP, 인터넷 프로토콜 역할
- 지정한 IP 주소(IP Address)에 데이터 전달
- 패킷(Packet)이라는 통신 단위로 데이터 전달
- IP 패킷 정보 : 출발지 IP, 목적지 IP, 전송 데이터 등
- 클라이언트 패킷 전달
- 서버 패킷 전달
#### IP 프로토콜의 한계
- 비연결성 : 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
- 비신뢰성 : 중간에 패킷이 사라지면? 패킷이 순서대로 안 오면?
- 프로그램 구분 : 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이면?

### TCP, UDP
#### 인터넷 프로토콜 스택의 4계층
- 애플리케이션 계층 - HTTP, FTP
- 전송 계층 - TCP, UDP
- 인터넷 계층 - IP
- 네트워크 인터페이스 계층
#### 프로토콜 계층
1. 프로그램이 `Hello, world!` 메시지 작성
2. SOCKET 라이브러리를 통해 전달
3. TCP 정보 생성, 메시지 데이터 포함
4. IP 패킷 생성, TCP 데이터 포함
- 즉 Ethernet frame 안에 IP 안에 TCP 안에 HTTP
#### TCP 패킷 정보
- 출발지 PORT, 목적지 PORT, 전송 제어, 순서, 검증 정보 등
#### TCP 특징
- 전송 제어 프로토콜(Transmission Control Protocol)
- 연결 지향 - TCP 3 way handshake (가상 연결, 논리적인 연결)
  1. SYN(접속 요청) `클라이언트 -> 서버`
  2. SYN + ACK(요청 수락) `클라이언트 <- 서버`
  3. ACK `클라이언트 -> 서버`
  4. 데이터 전송
  - 1~3을 connect, 연결 과정이라고 함
  - 3에서 ACK와 함께 데이터 전송 가능
- 데이터 전달 보증
  1. 데이터 전송
  2. 데이터 잘 받았다는 응답을 내려줌
- 순서 보장
  - 잘못된 순서가 오면 잘못된 순서부터 다시 보내라고 응답
- 신뢰할 수 있는 프로토콜
- 현재는 대부분 TCP 사용
#### UDP
- 사용자 데이터그램 프로토콜(User Datagram Protocol)
- 하얀 도화지에 비유(기능이 거의 없음)
- 연결 지향 X
- 데이터 전달 보증 X
- 순서 보장 X
- 데이터 전달 및 순서가 보장 되지 않지만, 단순하고 빠름
- 정리
  - IP와 거의 같다. + PORT, 체크섬 정도만 추가
  - 애플리케이션에서 추가 작업 필요

### PORT
- 한번에 둘 이상 연결해야 한다면?
- 포트란 같은 IP 내에서 프로세스를 구분시켜 주는 것
- 0 ~ 65535 할당 가능
- 0 ~ 1023 : 잘 알려진 포트, 사용하지 않는 것이 좋음
  - FTP - 20, 21
  - TELNET - 23
  - HTTP - 80
  - HTTPS - 443

### DNS
- IP는 기억하기 어렵다.
- IP는 변경될 수 있다.
- DNS = 도메인 네임 시스템(Domain Name System)
- 전화번호부
- 도메인 명을 IP 주소로 변환

## URI와 웹 브라우저 요청 흐름
### URI(Uniform Resource Identifier)
- URI는 로케이터(Locator), 이름(Name) 또는 둘 다 추가로 분류될 수 있다.
- URL(Uniform Resource Locator), URN(Uniform Resource Name) `in` URI(Uniform Resource Identifier)
- URI
  - Uniform : 리소스를 식별하는 통일된 방식
  - Resource : 자원, URI로 식별할 수 있는 모든 것(제한 없음)
  - Identifier : 다른 항목과 구분하는데 필요한 정보
- URL, URN
  - URL - Locator : 리소스가 있는 위치를 지정
  - URN - Name : 리소스에 이름을 부여
  - 위치는 변할 수 있지만, 이름은 변하지 않는다.
  - URN 이름만으로 실세 리소스를 찾을 수 있는 방법이 보편화 되지 않음
  - URL이 보편적
 
 #### URL 전체 문법
- 예시 : `https://www.google.com:443/search?q=hello&hl=ko`
- 프로토콜(http) + 호스트명(www.google.com) + 포트번호(443) + 패스(/search) + 쿼리 파라미터(q=hello&hl=ko)
- `scheme://[userinfo@]host[:port][/path][?query][#fragment]`
  - scheme
    - 주로 프로토콜 사용
    - 프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙 (http, https, ftp 등)
    - http는 80 포트, https는 443 포트를 주로 사용, 포트는 생략 가능
    - https는 http에 보안 추가한 것 (HTTP Secure)
  - userinfo
    - URL에 사용자 정보를 포함해서 인증
    - 거의 사용하지 않음
  - host
    - 호스트명
    - 도메인명 또는 IP 주소를 직접 사용가능
  - port
    - 접속 포트
    - 일반적으로 생략, 생략시 http는 80, https는 443
  - path
    - 리소스 경로(path), 계층적 구조
  - query
    - key=value 형태
    - ?로 시작, &로 주가 가능
    - query parameter, query string 등으로 불림, 웹 서버에 제공하는 파라미터, 문자 형태
  - fragment
    - html 내부 북마크 등에 사용
    - 서버에 전송하는 정보는 아님

### 웹 브라우저 요청 흐름
1. DNS 조회
2. HTTP 요청 메시지 생성 - SOCKET 라이브러리를 통해 전달 - TCP/IP 패킷 생성, HTTP 메시지 포함
```
GET /search?q=hello&hl=ko HTTP/1.1
Host: www.google.com
```
3. HTTP 메시지 전송
4. 클라이언트에서 서버로 요청 패킷 전달
5. 서버에 요청 패킷 도착
6. TCP/IP 패킷 안의 HTTP 메시지 해석
7. HTTP 응답 메시지 생성
8. 서버에서 클라이언트로 응답 패킷 전달
9. 클라이언트에 요청 패킷 도착
10. HTML 렌더링 등의 요청 결과를 보여줌
