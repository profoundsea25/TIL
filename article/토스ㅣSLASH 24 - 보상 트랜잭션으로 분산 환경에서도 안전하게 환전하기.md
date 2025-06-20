# [토스ㅣSLASH 24 - 보상 트랜잭션으로 분산 환경에서도 안전하게 환전하기](https://www.youtube.com/watch?v=xpwRTu47fqY)

### 기술적 어려움: 분산 환경 트랜잭션
- 초기 코어뱅킹 시스템은 모놀리식. 이를 MSA로 전환 중
- 외화 계좌 같은 신규 상품은 새롭게 프로젝트를 구축
    - 기존 시스템을 쓰는 원화 계좌
    - 신규 시스템을 쓰는 외화 계좌
- 환전할 떄 원화 계좌에서 돈을 빼고, 외화 계좌에 돈을 넣어야 한다.
    - 이것이 하나의 애플리케이션과 하나의 DB에서 발생하지 않기 때문에, 간단하게 트랜잭션 구현이 쉽지 않음.
- 높은 가용성을 위해 Orchestration Saga 패턴 선택
    - 중간 트랜잭션 상황을 추적하기 쉬운 장점
    - but orchestration 서버에 의존성이 생기며, 단일 장애 지점이 될 수 있음.
- 문제가 되는 것은 서버 에러(500 Error), 네트워크 에러, 메시지 produce/consume 실패
- 트랜잭션은 출금부터 진행한다. 출금 취소가 입금 취소보다 더 쉽기 때문이다.
- (해피 패스의) 출금&입금에는 동기로 구현.
    - 사용자가 결과를 바로 알 수 있어야 하고, 출금 다음에 입금을 진행해야 하기 때문
- (에러 발생 후) 출금 취소는 비동기(Messaging)
    - 유저가 기다릴 필요가 없고, 출금 취소 에러 핸들링이 필요없기 때문 (결과적 정합성만 보장하면 됨.)
- 원화 계좌 출금에서 타임아웃이 발생한 경우, 결과를 조회하여 그에 맞게 처리.
    - 원화 계좌 출금이 정상 처리 되었다면 출금 취소 처리
    - 원화 계좌 출금이 처리되지 않았다면 환전 실패 처리
- 출금 결과 확인도 실패했다면?
    - 메시지를 지연시켜 발생하는 카프카 메시지 스케쥴러를 사용.
        - 에러가 발생한 서버에 곧바로 요청을 또 보내면 에러가 다시 발생할 가능성이 크다.
        - 그래서 메시지를 나중에 다시 consume 할 수 있도록 늦게 발행하는 카프카 메시지 스케줄러를 활용.
    - 지연시켜서 발행한 메시지가 또 에러가 난다면, 시간을 더 늘려서 발행하도록 한다.
    - 재시도 처리 횟수에 도달하면 알림을 보내 개발자가 직접 수동으로 메시지를 발행할 수 있게 한다.
- Orchestration 서버가 다운된다면?
    - Orchestration 서버는 중간 트랜잭션 상황을 저장하기 때문에, 그것을 기반으로 배치를 실행시켜 중단된 상태부터 재처리 할 수 있다.
- 원화 출금 취소가 실패한다면?
    - Dead Letter Queue에 메시지 발행.
    - DLQ도 실패하면 개발자가 수동으로 할 수 있도록 처리.
    - 결과적 일관성 구현
- 트랜잭션은 성공했지만 메시지 발행(produce)이 실패한다면?
    - Produce Dead Letter Queue 사용
    - 기존 서비스 메시지 브로커가 회복되면, PDL에서 메시지를 발행하여 처리를 시도한다.
- 모든 거래의 이벤트를 기록한다.
- 원화 원장과 외화 원장에 기록된 데이터를 정보계 DB에 넣어 대차가 맞는지 확인. 