# JPQL - 벌크 연산
> 김영한님의 [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard) 강의 내용 정리입니다.

#### 벌크 연산
- 쿼리 한 번으로 여러 테이블 Row를 변경
- `excuteUpdate()`의 결과는 영향받은 엔티티 수 반환
- UPDATE, DELETE 지원
- INSERT(insert into ... select, 하이버네이트에서 지원)
- 실행시 EntityManager의 `flush()` 메서드 자동 호출
```java
String qlString = "update Product p set p.price = p.price*1.1 where p.stockAmount < :stockAmount";

// update한 수 출력
int resultCount = em.createQuery(qlString).setParameter("stockAmount", 10).executeUpdate();
```
#### 벌크 연산 주의
- 벌크 연산은 영속성 컨텍스트를 무시하고 데이터베이스에 직접 쿼리를 넣는다. 영속성 컨텍스트에 바로 반영이 안 된다.
- 따라서 오류 발생 가능성이 있음
- 해결 방법
  - 1. 벌크 연산을 먼저 실행하기
  - 2. (권장) 벌크 연산 수행 후 영속성 컨텍스트만 초기화 (벌크 연산 코드 다음 `em.clear()` 수행하기)
