# [Node.js가 싱글스레드 서버라는 미신(feat. Node.js의 대용량 데이터 처리)](https://medium.com/naverfinancial/node-js%EA%B0%80-%EC%8B%B1%EA%B8%80%EC%8A%A4%EB%A0%88%EB%93%9C-%EC%84%9C%EB%B2%84%EB%9D%BC%EB%8A%94-%EB%AF%B8%EC%8B%A0-feat-node-js%EC%9D%98-%EB%8C%80%EC%9A%A9%EB%9F%89-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%B2%98%EB%A6%AC-cf1d651290be)

- Node.js는 자바스크립트 코드를 실행할 떄, 단 하나의 메인 스레드를 사용한다. 이 안에서 이벤트 루프가 돌고 있다.
  - 그러나 이 메인스레드는 오래 걸리는 작업은 직접 처리하지 않는다.
  - 파일 I/O, 네트워크 I/O 등 I/O 작업을 `libuv`라는 C로 작성된 라이브러리로 넘긴다.
  - `libuv`는 내부의 워커 스레드풀에 태스크를 맡기고, 메인 스레드는 다시 이벤트 루프를 돌며 다른 작업을 처리한다.
  - I/O 작업이 완료되면 작업 결과가 담긴 콜백이 이벤트 큐에 쌓이고, call stack이 비었을 떄 V8 엔진이 그 콜백을 실행한다.
- 따라서 Node.js는 멀티스레드 아키텍처 기반 서버다.
- 작업은 병렬로 처리되지만, 콜백은 순차적으로 메인 스레드에서 실행된다.
- `libuv` 워크 스레드 풀이 병렬로 작업을 수행하지만, 이 스레드들이 우리가 작성한 자바스크립트 코드 내부의 상태나 메모리에 직접 접근하지는 않는다.
  - 워커 스레드는 "작업 위임 대상"일 뿐이다.
- 실제로 실행 흐름을 제어하거나 변수에 접근하는 주체는 오직 메인스레드 하나다.
- Node.js는 가볍고 빠르지만, 진짜 무거운 CPU 작업이 들어오면 조용하게 멈춰버리는 구조다.
  - Java Spring은 스레드풀을 사용하기 때문에 더 잘 처리할 수 있다.
- Node.js의 장점
  - 컨테이너 환경에서의 가벼움
  - 스케일아웃에 최적화된 구조
    - 기본적으로 필요한 메모리가 작다.
    - 메인스레드 하나만 사용하기 때문에, 명확한 단위론 수평 확장이 가능하다. (CPU 설정이 용이하다.)
  - I/O 중심 처리에 강한 런타임 특성
- Node.js의 단점
  - 트랜잭션 처리
  - 계층적 설계
  - ORM
  - 생명주기 제어
  - 멀티스레드
- 따라서 Node.js는 필요한 만큼만 만들고, 명확하게 쪼개어 확장하기엔 좋은 선택지다.
- 활용 사례 : 네이버페이 타임라인 서버
  - Kafka에서 결제/구매 데이터를 받아, 내부 컨버팅 로직을 거쳐 MongoDB에 저장하고, 이를 API로 호출한다.
  - 60억 건을 마이그레이션 할 때, 수평 확장을 통해 해결했다.
  - 지금도 초당 몇 백의 TPS를 잘 버텨내고 있다.

### Comment
- Node.js를 왜 사용할까?의 답을 명쾌하게 제시해준 글이라고 생각한다.
- 컨테이너 환경에서 빠른 수평 확장과 I/O 중심의 처리가 중요하다면 node.js가 좋은 선택지
