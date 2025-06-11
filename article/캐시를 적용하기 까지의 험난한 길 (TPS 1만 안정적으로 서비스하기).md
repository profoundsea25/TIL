# [캐시를 적용하기 까지의 험난한 길 (TPS 1만 안정적으로 서비스하기)](https://toss.tech/article/34481)

- Strong Consistency 구현
  - DB Commit 직후 모든 읽기 요청은 변경 사항을 응답해야 함
- DB 부하가 높아 캐시를 도입
- DB Commit -> `@TransactionalEventListener`를 통해 Cache Evict
- 정책을 "DB 커밋 직후"가 아닌 "API가 처리가 완료된 후"로 바꾸어, DB와 캐시 동기화 사이에 발생할 수 있는 비일관성을 허용

### Comment
- 일관성 문제는 빈틈을 막는 일이다. 이기종 간 데이터 동기화는 비일관성이 발생할 수 밖에 없는데, 그것을 어느 정도로 허용하느냐의 문제.
- `@TransactionalEventListener`, `@EntityListener` 레퍼런스 참고.