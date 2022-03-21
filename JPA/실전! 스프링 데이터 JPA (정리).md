> 김영한님의 [실전! 스프링 데이터 JPA](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-JPA-%EC%8B%A4%EC%A0%84) 강의 내용 정리입니다.

### 공통 인터페이스
- `extends JpaRepository<T, ID>`
  - T : 엔티티 타입
  - ID : 식별자 타입 (PK)
- 주요 메서드
  - `save(S)` : 새로운 엔티티는 저장하고 이미 있는 엔티티는 병합한다.
  - `delete(T)` : 엔티티 하나를 삭제한다. 내부에서 `EntityManager.remove()` 호출
  - `findById(ID)` : 엔티티 하나를 조회한다. 내부에서 `EntityManager.find()` 호출
  - `getOne(ID)` : 엔티티를 프록시로 조회한다. 내부에서 `EntityManager.getReference()` 호출
  - `findAll(...)` : 모든 엔티티를 조회한다. 정렬이나 페이징 조건을 파라미터로 제공할 수 있다.

### 메소드 이름으로 쿼리 생성
- 조회 : find...By, read...By, query...By, get...By
- COUNT : count...By 반환타입 long
- EXISTS : exists...By 반환타입 boolean
- 삭제 : delete...By, remove...By 반환타입 long
- DISTINCT : findDistinct, findMemberDistinctBy
- LIMIT : findFirst3, findFirst, findTop, findTop3
- 엔티티으 필드명이 변경되면 인터페이스에 정의한 메서드 이름도 꼭 함께 변경해야 한다. 그렇지 않을 경우 애플리케이션 시작하는 시점에 오류 발생

### @Query, 리포지토리 메소드에 쿼리 정의하기
- JPA NamedQuery처럼 애플리케이션 실행 시점에 문법 오류를 발견할 수 있다.
- 실무에서 메소드 이름으로 쿼리 생성 기능은 파라미터가 증가하면 메서드 이름이 매우 지저분해지므로, `@Query` 기능을 자주 사용하게 된다.
