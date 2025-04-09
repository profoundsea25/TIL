# [Mastering Data Consistency Across Microservices](https://blog.bytebytego.com/p/mastering-data-consistency-across)
- 마이크로서비스에서 데이터 일관성 마스터하기

- 마이크로서비스 아키텍처에서, 각각의 서비스들은 독립적으로 운영되고, 배포되고, 확장할 수 있음.
- 그러나 이런 분리 때문에, 데이터 일관성에서 문제가 발생할 수 있음.
- 데이터 일관성이란, 모든 서비스에서 같은 시점에는 같은 데이터를 보장해야 함을 의미한다.

### 일관성 모델
1. Strong Consistency
  - 모든 노드에서 업데이트된 데이터를 트랜잭션 커밋 이후 즉시 볼 수 있어야 한다.
  - 모든 읽기 연산은 가장 최근의 쓰기 연산의 결과를 읽어야 한다.
  - 금융 거래나 재고 관리에 필수.
  - 성능과 가용성(데이터를 소유한 어느 서버라도 이상이 있으면 안 됨.) 문제를 해결해야 함.
2. Eventual Consistency
  - 모든 노드가 "결국에는" 같은 데이터를 보게 된다. (즉시가 아님.)
  - 변경사항이 비동기적으로 전파됨.
  - 일시적으로는 다른 노드끼리 다른 결과를 나타낼 수 있음.
  - 약간의 차이가 발생해도 괜찮을 때 사용
  - 다른 서비스에서 변경 중 충돌이 발생할 수 있음.
3. Casual Consistency
  - 연산의 순서가 보장됨. (인과관계를 보장함.)
  - 그러나 Strong Consistency처럼 글로벌 동기화가 안 될 수 있음.
  - 구현이 복잡하고, 높은 지연시간 문제를 해결해야 함.
4. Read-Your-Writes Consistency
  - 자신의 업데이트는 즉시 볼 수 있지만, 다른 사용자는 그것을 볼 수 없을수도 있음.
  - ex. 소셜미디어에 피드를 쓰면, 본인은 바로 볼 수 있지만 타인이 볼 때까지 시간이 걸릴 수 있음.

### 적절한 일관성 모델 선택하기
- 실시간 정확성이 아주 중요하다면, Strong Consistency
  - 뱅킹, 재고 관리
- 실시간 정확성보다 속도와 가용성이 중요하다면, Eventual Consistency
  - 소셜 미디어, 분석 대시보드
- 순서가 중요하다면, Casual Consistency
  - 채팅 애플리케이션
- 자신의 변경사항을 바로 보아야 한다면, Read-Your-Write Consistency
  - 쇼핑 장바구니, 프로필 변경

### 데이터 일관성 보장을 위한 전략
1. 동기 vs 비동기 통신
  - 동기: 요청 후 응답까지 기다린다.
    - 응답 시간 지연, 가용성 하락, 시스템 간 강결합 발생
  - 비동기: 요청 후 응답을 기다리지 않고, 다음 작업을 진행한다.
    - 일시적으로 데이터 불일치가 발생할 수 있다.
2. 코레오그라피와 오케스트레이션
  - 코레오그라피
    - 중앙 관리자(central controller) 없이 이벤트 통신
    - 중간 과정에 실패가 발생할 경우, 그에 대한 보상 이벤트가 해당 서비스에서 발행된다.
  - 오케스트레이션
    - 중앙 오케스트레이터(central orchestrator) 서비스 간 통신을 관리
    - 중간 과정에 실패가 발생할 경우, 그에 대한 보상 이벤트가 중앙을 통해 발행된다.
3. 이벤트 드리븐 아키텍처
  - 서비스간 결합도를 낮추고, 각각 확장 가능하도록 만든다.
  - 상태 변화가 있을 경우 서비스에서 이벤트를 발행한다.
    - 동기 API를 통한 직접 통신을 하지 않는다.
  - "이벤트" 기반으로 통신한다.
  - 일시적으로 데이터가 불일치 할 수 있다.
  - 하나의 서비스가 다운되더라도, 이벤트는 큐에 있어 나중에 처리할 수 있다. (유실 방지)
  - 서비스가 독립적으로 운영되기 때문에, 높은 처리량을 보일 수 있다.
  - CQRS
    - 쓰기 연산과 읽기 연산을 분리
4. 데이터베이스 레벨 솔루션
  - 분산된 환경에서도 일관성 메커니즘을 가진 DB를 사용
  - 글로벌 트랜잭션 지원
  - 필요하다면 Strong Consistency도 가능
  - ex. Google Spanner, Cockroach
  - CDC(Change Data Capture)
    - 데이터베이스의 변경사항을 추적하고 이벤트를 발행
    - 로그 기반

### Best Practice
1. 적절한 Consistency Model을 선택하기
  - Strong Consistency, Eventual Consistency ...
2. Handling Failures Gracefully
  - 마이크로서비스 아키텍처에서 오류는 피할 수 없다.
  - 대처 방법
    - 재시도(Retry)로 이벤트 유실 방지
    - 서킷 브레이커로 과부하 방지
    - Dead Letter Queues(DLQ)를 만들어 데이터 유실 방지
3. Versioning and Schema Evolution
  - Schema Versioning: 이전 데이터와 새 데이터를 동시에 볼 수 있음.
  - Feature Toggle: 데이터베이스 변경 사항을 점진적으로 배포할 수 있게 하여 즉각적인 업데이트와 관련된 위험을 감소
4. Observability and Monitoring
  - 시스템의 행동에 대해 visibility를 잘 유지해야 비일관성을 빠르게 인지할 수 있음.
  - Distributed tracing: 요청이 마이크로서비스를 통과한 흐름을 볼 수 있음.
  - Log aggregation and monitoring: 시스템 상태와 데이터 이상 감지
