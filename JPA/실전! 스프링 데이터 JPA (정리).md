> 김영한님의 [실전! 스프링 데이터 JPA](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-JPA-%EC%8B%A4%EC%A0%84) 강의 내용 정리입니다.

#
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

#
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
- 벌크성 수정, 삭제 쿼리는 `@Modifying` 어노테이션을 사용
  - 사용하지 않으면 `QueryExecutionRequestException` 발생
- 벌크성 쿼리를 실행하고 나서 영속성 컨텍스트 초기화 `@Modifying(clearAutomatically = true)` (기본값 false)
  - 이 옵션이 없으면 영속성 컨텍스트에 과거 값이 남아서 문제가 될 수 있다. 수정 쿼리 이후 조회해야 한다면 반드시 영속성 컨텍스트를 초기화하자.
- 벌크 연산은 영속성 컨텍스트를 무시하고 실행하기 때문에, 영속성 컨텍스트에 있는 엔티티의 상태와 DB의 엔티티 상태가 달라질 수 있다.
  - 따라서 영속성 컨텍스트에 엔티티가 없는 상태에서 벌크 연산을 먼저 실행하고,
  - 부득이하게 영속성 컨텍스트에 엔티티가 이쓰면 벌크 연산 직후 영속성 컨텍스트를 초기화 한다. 

### @EntityGraph
- 연관된 엔티티들을 SQL 한번에 조회하는 방법
- 지연로딩 관계에 있으면, (`@ManyToOne` 등) 해당 객체를 조회할 때마다 쿼리가 실행된다. (N+1 문제 발생)
- 연관된 엔티티를 한번에 조회하려면 페치 조인이 필요하다.
- 이때 스프링 데이터 JPA는 JPA가 제공하는 엔티티 그래프 기능을 편리하게 사용하게 도와준다. 이 기능을 사용하면 JPQL 없이 페치 조인을 사용할 수 있다. (JPQL + 엔티티 그래프도 가능)
- 사실상 페치 조인(FETCH JOIN)의 간편 버전
- LEFT OUTER JOIN 사용

### JPA Hint & Lock
#### JPA Hint
- JPA 쿼리 힌트(SQL 힌트가 아니라 JPA 구현체에게 제공하는 힌트)
- QueryHint로 readOnly 상태 설정 가능
- Page를 사용할 때 `forCounting`는 추가로 호출하는 페이징을 위한 count 쿼리도 쿼리 힌트 적용 
#### Lock
- 따로 공부하기

#
### 사용자 정의 리포지토리 구현
- 스프링 데이터 JPA 리포지토리는 인터페이스만 정의하고 구현체는 스프링이 자동 생성
- 인터페이스의 메서드를 직접 구현하고 싶다면, 구현 클래스 이름을 리포지토리 인터페이스 이름 + Impl
  - 스프링 데이터 JPA가 인식해서 스프링 빈으로 등록한다.
- Impl 대신 다른 이름으로 변경하고 싶으면 XML이나 JavaConfig 설정을 건드리면 된다.
- 실무에서는 주로 QueryDSL이나 SpringJdbcTemplate을 함께 사용할 때 사용자 정의 리포지토리 기능을 자주 사용한다.
- 항상 사용자 정의 리포지토리가 필요한 것은 아님. 그냥 임의의 리포지토리를 만들어도 되며, 이때는 스프링 데이터 JPA와 관계없이 별로도 작동.
- 최근에는 사용자 정의 인터페이스 명 + Impl 방식도 지원한다. (소스코드에서 `MemberRepositoryImpl` 대신에 `MemberRepositoryCustomImpl`같이 구현해도 됨, 더 직관적이기 때문에 권장)

### Auditing
- 엔티티를 생성, 변경할 때 변경한 사람과 시간을 추적하고 싶을 때 사용
- JPA 주요 이벤트 어노테이션
  - `@PrePersist`, `@PostPersist`
  - `@PreUpdate`, `@PostUpdate`

#### 스프링 데이터 JPA 사용
- `@EnableJpaAuditing` : 스프링 부트 설정 클래스에 적용
- `@EntityListeners(AuditingEntityListener.class)` : 엔티티에 적용
- `@CreatedDate` `@LastModifiedDate` `@CreatedBy` `@LastModifiedBy`

### Web 확장 - 도메인 클래스 컨버터
- HTTP 파라미터로 넘어온 엔티티의 아이디로 엔티티 객체를 찾아서 바인딩
- 예를 들어 HTTP 요청은 회원 id를 받지만 도메인 클래스 컨버터가 중간에 동작해서 회원 엔티티를 반환
- 도메인 클래스 컨버터도 리파지토리를 사용해서 엔티티를 찾음
- 트랜잭션이 없는 범위에서 엔티티를 조회했으므로, 엔티티를 변경해서 DB에 반영되지 않는다. 따라서 이 엔티티는 단순 조회용으로만 사용해야 한다.

### Web 확장 - 페이징과 정렬
- 컨트롤러 파라미터로 `Pageable`을 받을 수 있다.
- 요청 파라미터로 여러 기능을 사용할 수 있다.
  - `/members?page=0&size3&sort=id,desc&sort=username,desc`
  - page : 현재 페이지, 0부터 시작한다.
  - size : 한 페이지에 노출할 데이터 건수. 기본값은 20
  - sort : 정렬 조건을 정의한다. asc는 생략 가능
- 페이징 정보가 둘 이상이면 접두사로 구분
  - `@Qualifier`에 접두사명 추가 "접두사명_XXX"
  - 예) `/members?meber_page=0&order_page=1`
```java
public String list(
  @Qualifier("member") Pageable memberPageable,
  @Qualifier("order") Pageable orderPageable, ...
```
