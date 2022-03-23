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

### 반환 타입
- 스프링 데이터 JPA는 유연한 반환 타입을 지원
- 조회 결과가 많거나 없을 경우
  - 컬렉션 
    - 결과 없음 : 빈 컬렉션 반환
  - 단건 조회
    - 결과 없음 : null 반환
    - 결과가 2건 이상 : `NonUniqueResultException` 예외 발생
- 참고로 단건으로 지정한 메서드를 호출하면 스프링 데이터 JPA는 내부에서 JPQL의 `getSingleResult()` 메서드를 호출한다.
- 이 메서드를 호출했을 때 조회 결과가 없으면 `NoResultException`예외가 발생하는데, 스프링 데이터 JPA는 단건 조회를 할 때 이 예외가 발생하면 예외를 무시하고 대신에 null을 반환한다.

### 스프링 데이터 JPA 페이징과 정렬
- `Pageable`은 인터페이스이다. 실제 사용할 때는 해당 인터페이스를 구현한 `PageRequest` 객체를 사용한다.
- `PageRequest(현재 페이지, 조회할 데이터 수, 정렬 정보)` \*페이지는 0부터 시작
- Slice (count X) 추가로 limit +1을 조회한다. 그래서 다음 페이지 여부를 확인한다.
- List (count X)
- 카운트 쿼리 분리가 실무에서 매우 중요하다. 복잡한 sql에서 사용하는데, 데이터는 left join을 하되 카운트는 left join을 안해도 됨을 이용하는 것이다.
- 전체 count 쿼리는 매우 무거우므로, 상황에 맞춰 카운트 쿼리를 잘 분리하자.
#### 페이징과 정렬 파라미터
- `org.springframework.data.domain.Sort` : 정렬 기능
- `org.springframework.data.domain.Pageable` : 페이징 기능 (내부에 Sort 포함)
#### 특별한 반환 타입
- `org.springframework.data.domain.Page` : 추가 count 쿼리 결과를 포함하는 페이징
- `org.springframework.data.domain.Slice` : 추가 count 쿼리 없이 다음 페이지만 확인 가능 (내부적으로 limit + 1 조회)
- `List`(자바 컬렉션) : 추가 count 쿼리 없이 결과만 반환

### 벌크성 수정 쿼리
38~
