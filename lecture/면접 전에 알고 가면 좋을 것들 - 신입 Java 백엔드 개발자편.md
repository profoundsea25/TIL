# [면접 전에 알고 가면 좋을 것들 - 신입 Java 백엔드 개발자편](https://www.inflearn.com/course/%EB%A9%B4%EC%A0%91-%EC%8B%A0%EC%9E%85-java-%EB%B0%B1%EC%95%A4%EB%93%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90)
- 강사: 널널한 개발자

---

- OS, Network 지식은 많이 알수록 좋다.
  - 인프라와 직결되기 때문
  - AWS, GCP 등 설정할 때 도움이 된다.
- JVM, GC 지식은 "운영을 고려한 개발"을 하는데 직결된다.

### 웹을 이루는 3대 요소
1. HTTP + HTML(+CSS)
2. WAS
3. DB

### URL와 URI
- Uniform Resourse Identifier
- Uniform Resourse Locator
- URI > URL (URI가 URL을 포함한다.)
- 리소스(Resource)를 식별하기 위함 
- {hostname}.{domain}
  - www.a.com에서, www는 host, a.com은 도메인

### HTTP
- Request - Response 구조 
  - 항상 응답코드를 가지고 응답한다.
- ASCII 코드, 즉 문자열 기반 프로토콜 
- method 
  - 대부분 GET, POST 
  - PUT, DELETE는 보안 상의 이유로 차단하는 경우가 있음.

### 웹 서비스 기본 구조
- 흐름과 구조는 알아야 한다.
- Web Client <-> Web server <-> WAS <-> DB

### 인터넷의 핵심 기술
- HTTP 
  - Client/Server 구조 
  - = Request/Response 구조
  - stateless (무상태성) = 지속적인 연결을 하지 않는다.
- HTML 
  - tag 기반 
- IP = Internet Protocol

### 유지보수가 쉬우려면
- 영역을 분리한다.
  - 데이터 영역
  - 비즈니스 로직
  - UI

### 웹 서비스 구조
- 프론트
  - HTML, CSS, 이미지, JavaScript 순서로 응답 
  - 웹 브라우저 구동 순서
    1. 구문 분석
    2. 렌더링
    3. JS 엔진
  - JavaScript의 소스는 서버에 저장되어 있지만, 실행은 클라이언트에서 한다.
    - 이는 CORS와 연결된다.
  - Cookie
    - key-value 구조
    - 서버와 상호작용을 위해 클라이언트에서 사용
- 백엔드 개발자라면, 사용자 입력은 절대 신뢰하면 안되며, 반드시 검증해야 한다.
- 최근에는 클라이언트 다양화에 따라, 백엔드 서버가 데이터만 응답하는 추세
  - 백엔드에서 View에 대한 의존성을 줄임 
  - 따라서, 백엔드가 HTML을 응답하지 않고, 프론트엔드에서 HTML을 생성 
    - 그 툴들이 바로 React.js, Vue.js 
  - 이는 프론트엔드에서 백엔드로의 요청이 I/O 관련이 된다. 
    - 흔히 말하는 CRUD
    - 그래서 백엔드는 그에 맞는 API를 제공하게 되며 RESTful API가 보편화되었다. 
  - 결국 DB가 느려지면 다 느려지기에, 장애대응 측면에서 DB 응답시간 모니터링이 중요하다.
- APM (application performance monitoring)
  - JVM, DB 등의 응답속도, 상태 등을 확인하는데 필요 
  - 솔루션
    - 제니퍼
    - Scouter
- 웹을 이루는 3-Tier
  - Web Server
  - Web Application Server 
  - Database
- 보안 3요소 
  - IPS (1차 보안 체계)
  - SSL 
  - WAF (Web Application Firewall) (2차 보안 체계)
    - Web Server 뒤로 가기도 한다.
- DMZ
  - 외부에서 접속 가능한 구간 
  - 방화벽(WAF) 직전까지의 구간
- ISMS-P 인증
  - 2차 보안 체계(ISP, WAF)가 있어야 인증을 받을 수 있다.
- 백엔드는 "운영"을 고민해서 개발해야 한다.

### 운영체제 구조 (User Mode와 Kernel Mode 그리고 JVM)
- OS의 핵심 기능
  - multi tasking를 효율적으로 하기 위해 하드웨어를 제어
    - 동기화 등
  - RAM, SSD 등을 가상 메모리로 처리
- 플랫폼 = H/W + Kernel Mode
- Native Code = CPU, OS에 의존적인 코드
  - C, C++ 등
- Managed Code = CPU, OS등에 의존적이지 않은 코드
  - Java 등
- Virtual Machine
  - OS, Memory, CPU 등의 역할을 함
  - 버츄얼 머신 자체는 플랫폼 종속적이지만, 그 버츄얼 머신이 실행시키는 코드는 플랫폼 의존적이지 않음
- 네트워크 입출력을 위한 파일을 특별히 socket이라고 한다.
- 공부할 것
  - Big endian System

### 브라우저에 URL을 입력하면 일어나는 일을 5분 이상 말하기
- 굉장히 포괄적인 질문
1. URL 입력 
2. URL을 IP주소로 변환
   - HTTP 통신을 하려면, TCP 연결이 선행되어야 한다.
   - 그래서 IP 주소가 필요하다.
   - 2-1. 로컬의 hosts 파일 내용 확인
   - 2-2. DNS 캐시 확인 (메모리)
   - 2-3. DNS 질의 (+캐싱)
3. TCP/IP 연결
4. HTTP Request
5. HTTP Response
   - 응답 받은 내용을 Disk에 캐싱

### DNS 
- Root DNS (전세계 13대)부터 하위 DNS들에게 질의를 계속 해본다. 
- root DNS -> com을 다루는 DNS -> www라는 호스트 있니? -> ..

### HTTP 버전별 특징
- HTTP 1.0
  - 모든 요청을 TCP 커넥션부터 처리
- HTTP 1.1
  - TCP 커넥션을 중복 처리하지 않음
  - 순차적으로 처리
- HTTP/2
  - wait에 대한 대기가 없음.
  - 바이너리 프레이밍 레이어
    - 프레임 단위로 나눠서 보낸다.
    - 요청/응답을 프레임 단위로 분할하여 처리 가능하다. -> 극적인 속도 향상
  - 서버 푸시가 가능하다.
  - 헤더 압축
    - 직전 요청과 헤더에 중복되는 내용을 제외하고 요청한다.
- HTTP/3
  - TCP가 아닌 UDP
    - TCP 전송 제어를 사용하지 않음
    - 다만, HTTP 계층의 QUIC이 대신 전송 제어를 수행
  - TLS가 필수

### 네트워크 보안 솔루션 종류별 대응 계층
- L3~L4 사이에서 작동하는 네트워크 장비
  - Inline (방화벽)
    - 패킷 필터링, IPS 등
    - HTTP 요청을 볼 수 있다.
    - HTTPS는 볼 수 없다.
  - Out Of Path (센서)
    - 패킷을 복사하여 분석 
- SSL 인증서를 L4 계층(IPS, LB) 수준에서 설치해야 할 수 있다.

### WAF와 Proxy 구조
- Proxy 구조
  - socket 수준
  - TCP 연결이 Proxy 개수만큼 추가된다.
- WAF(Web Application Firewall)
  - 방화벽은 Proxy의 일종
  - 사용자의 입력을 검증하는 역할
    - 요청의 parameter, file 등을 검증
  - 사용자는 웹서버의 실제 IP주소가 아닌, 방화벽의 IP로 접속한다.
    - 웹서버를 보호한다.
  - SSL 인증서가 배치됨

### SSL인증서는 어디에 설치되나?
- SSL 인증서
  - HTTPS를 하려면 SSL 인증서가 있어야 한다.
- PKI
  - key를 기반으로 평문을 암호문으로 변경
  - 암호화와 복호화 key가
    - 동일하면 대칭키
    - 동일하지 않으면 비대칭키
      - public key
      - private key
      - ex. RSA
- 클라이언트가 직접 최초로 통신하는 곳이 SSL 인증서가 설치되는 곳 (DMZ)
- SSL은 CA가 private key로 Hash 결과가 암호화된 내용을, 사전에 배포된 CA Public Key로 암호를 풀어 검증한다.

### JVM 기본 구조
- Class Loader
- Runtime Data Area
  - Method Area
  - Heap Area
    - 메모리를 가장 많이 차지 하면서, GC의 대상이기도 함. 중요
  - Stack Area
  - PC Register
  - Native Method Stack
- Execution Engine (Interpreter, JIT Compiler, GC)
- Native Method Interface (JNI)
- Native Method Library

### GC 원리
- Java 8 JVM Heap
  - Heap도 유한 자원이다.
  - GC는 결국 속도에 문제를 일으킨다.
  - Full GC, Over 등
- Metaspace
  - 로드되는 클래스, 메소드 등에 대한 메타 정보
  - Java Heap이 아닌 Native 메모리 영역 사용
    - 자바 7까지는 Permanent generation으로 heap에 있었음.
  - 리플렉션 클래스 로드 시 사용 (특히 Spring)
  - TPS 기반으로 Metaspace 크기를 잡아야 한다.
- JVM Heap 영역
  - New (Young generation)
  - 새로 생성한 개체가 사용하는 영역
  - Minor GC
  - Eden, From, To 로 구성
    - Eden
      - 객체 생성 직후 저장되는 영역
      - Minor GC 후 사용하는 객체는 Survivor 영역으로 이동
      - Copy & Scavenge 알고리즘
  - Survivor 0, 1
    - Minor GC 발생 시 Eden, S0에서 살아남은 객체는 S1으로 이동
    - S1에서 살아남은 객체는 Old로 이동 (특정 횟수 이상)
- Old (Old generation)
  - Young generation 영역에서 소멸하지 않고 남은 객체들이 사용하는 영역
  - Full GC 발생 시 개체 회수 -> 지연 발생
  - Mark & Compact 알고리즘
- JVM Garbage Collector
  - Heap에서 참조되지 않는 객체를 수집하여 제거, 메모리 회수
  - GC 수행 시 프로그램 일시 정지 가능
    - Minor GC가 보통 1초 이내
    - Full GC가 수 초 이상 될 수 있음
  - 관건 : 어떻게 하면 Full GC를 발생하지 않게 코딩할 수 있는가?

### GC와 JVM에 대해 묻는 의도는 따로 있다.
- GC에 관련된 것들을 공부하자. 개념, 장/단점 등
  - Serial GC, Parallel GC, G1(Garbage First) GC ...
- 신입을 교육시켜서 키우는 경우가 많이 줄어들었다.
- 서비스 런칭 경험이 없다면, 그와 비슷한 경험을 해야 한다.
- 꼭 해야 할 것 : 부하테스트
  - 모니터링 tool 설치(APM) & 부하테스트
  - WEB+WAS 서버, DB 서버, Client 서버, 모니터링 서버를 각각 따로 구현한다. 
  - 부하테스트를 시행 (JMeter 등)
  - APM (Scouter 등)으로 관측
  - 과부하로 인해 발생하는 문제를 인지
  - 문제에 대한 해결책 실행 (=개선)
  - 모든 과정은 "정량적"으로 설명하여 문서화
  - NPM까지 분석해내면 대박

### 인덱싱을 하는 이유는 무엇일까?
- DB = 자료구조를 제대로 공부하면 끝난다.
- 인덱스를 걸면 읽기는 빠르지만 쓰기는 느려질 수 있다.

### 트랜잭션
- 동시성 문제 해결이 핵심
- Select -> RAM에 적재 -> Commit -> File에 반영
- Rollback이 일어나면, RAM에 적재된 데이터를 폐기하고, File에 반영되지 않는다.
- SQL 잘 알아야 한다. 모르고 실무 한다는 건 말이 안 된다.

### DB 커넥션 풀의 용도와 필요를 묻는 숨은 의도?
- DBCP Connection Pool 연결 대기 지연 현상 글 읽기 (블로그)
- DB와의 새 연결은 시간이 많이 걸린다.
- connection 요청 대기 시간을 길게 설정하는 것도 좋지 않다. (장애가 될 수 있다.)
- 강제 회수도 적절치 않을 수 있다.
- 결국 중요한 것은 "운영 경험"이다. (문제 분석 및 적절한 해결)

### 온프레미스 -> 클라우드 장점과 단점
- 핵심 2개 : 비용, 가시성
- 답변 관점 : 기업 관점, 기술 관점
- 장점
  - 신속한 인프라 도입, 유연함
  - 트래픽 폭주 대응 (auto scaling)
  - 손쉬운 글로벌 서비스
  - 강력한 보안
  - 합리적인 비용
    - 인프라 초기 도입 비용 + 운영 비용을 하나로 관리하기 좋다.
    - 다만, 진짜 저렴한지는 따져봐야 한다. 
- 단점
  - 생각보다 비싼 비용
  - 커지는 클라우드 의존도
  - 데이터 보관의 불안함
  - 가시성 확보 이슈
    - 클라우드에서는 네트워크 미러링, 패킷 수집/분석 등이 안 되는 경우가 있다.
- 읽을 거리
  - 배민 - AWS에서 서버(EC2) 패킷 미러링 하기

### XSS, CSRF 공격에 대한 이해와 방어
- 쿠키를 해킹하면 ID/PW없어도 로그인이 가능하다.
- XSS (Cross-Site Scripting)
  - 게시물 내용에 JavaScript로 작성, 다른 사람이 읽으면. 그 게시물 속의 JS 내용이 실행되며 쿠키 값을 빼돌리는 것
  - 서버 쪽에서 입력(게시물 내용)을 실행되지 않도록 처리함으로써 방지
  - Java 시큐어 코딩 가이드 (정부 가이드) 참고
  - 회사가 다루는 개인정보량에 따라 지켜야 할 법이 달라지고, 감사가 나온다. 그에 대응할 시큐어 코딩은 기본적으로 알아야 한다.
- 추가적으로 공부할 것
  - Drive By Download 공격
  - CORS

### 서비스 기본 구조와 필수 고려사항 (최소한 이 정도)
- Q) 넷플릭스를 설계해보세요.
  - 사용자 인증 : 중복 접속을 어떻게 할 것인가?
  - 동영상 재생 : 다운로드 시도, 저장된 컨텐츠의 허용 기간 (-> DRM)
    - DRM을 적용하면 별도의 플레이어 설치가 필요할 수 있다.
  - 전송 방법 (HTTP? HLS?)
    - 코덱 선택
  - 부하 분산
