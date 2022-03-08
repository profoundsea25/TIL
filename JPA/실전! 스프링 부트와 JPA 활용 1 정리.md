> 김영한님의 [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1) 강의 내용 정리입니다.

## 엔티티 설계시 주의점
### 엔티티에는 가급적 Setter를 사용하지 말자
- Setter가 너무 많으면 변경 지점이 너무 많아서 어디서 데이터가 변경되었는지 추적하기 어려우며, 유지보수가 어렵다.
- 리팩토링으로 Setter는 제거하라

### 모든 연관관계는 지연로딩으로 설정하라
- 즉시로딩(`Eager`)은 예측이 어렵고, 어떤 SQL이 실행될지 추적하기 어렵다. 특히 JPQL을 실행할 때 N+1 문제가 자주 발생한다.
- 실무에서 모든 연관관계는 지연로딩(`LAZY`)으로 설정해야 한다.
- 연관된 엔티티를 함께 DB에서 조회해야 하면, `fetch join` 또는 엔티티 그래프 기능을 사용한다.
- `@XToOne`(OneToOne, ManyToOne) 관계는 기본이 즉시로딩이므로 직접 지연로딩으로 설정해야 한다.

### 컬렉션은 필드에서 초기화하자
- 컬렉션은 필드에서 바로 초기화 하는 것이 안전하다.
- `null`문제에서 안전하다.
- 하이버네이트는 엔티티를 영속화할 때, 컬렉션을 감싸서 하이버네이트가 제공하는 내장 컬렉션으로 변경한다. 만약 `getOrders()`처럼 임의의 메서드에서 컬렉션을 잘못 생성하면 하이버네이트 내부 메커니즘에 문제가 발생할 수 있따. 따라서 필드레벨에서 생성하는 것이 가장 안전하고, 코드도 간결하다.

## 테이블, 컬럼명 생성 전략
- 스프링부트에서 하이버네이트 기본 매핑 전략을 변경해서 실제 테이블 필드명은 다르다.
- 하이버네이트 기존 구현 : 엔티티의 필드명을 그대로 테이블의 컬럼명으로 사용 (`SpringPhysicalNamingStrategy`)
- 스프링부트 신규 설정(엔티티(필드) -> 테이블(컬럼))
  - 카멜 케이스 -> 언더스코어(memberPoint -> member_point)
  - .(점) -> _(언더스코어)
  - 대문자 -> 소문자
- 적용 2단계
  - 논리명 생성 : 명시적으로 컬럼, 테이블명을 직접 적지 않으면 `ImplicitNamingStrategy`사용
  - 물리명 적용 : 모든 논리명에 적용됨, 실제 테이블에 적용 (사용자가 원하는 임의의 이름으로 바꿀 수 있음, username -> usernm 등)

## 애플리케이션 아키텍처
### 계층형 구조
- controller : 웹 계층
- service : 비즈니스 로직, 트랜잭션 처리
- repository : JPA를 직접 사용하는 계층, 엔티티 매니저 사용
- domain : 엔티티가 모여 있는 계층, 모든 계층에서 사용
- 개발 순서 : 서비스, 리포지토리 계층 개발 -> 테스트 케이스를 작성해서 검증 -> 마지막에 웹 계층 적용

## 구현
- `@Repository` : 스프링 빈으로 등록, JPA 예외를 스프링 기반 예외로 변환
- `@PersistenceContext` : 엔티티 매니저(`EntityManager`) 주입
- `@PersistenceUnit` : 엔티티 매니저 팩토리(`EntityManagerFactory`) 주입
- `@Service` : 서비스 계층임을 나타냄. `@Component`와 거의 같다.
- `Transactional` : 트랜잭션, 영속성 컨텍스트
  - `readOnly = true` : 데이터의 변경이 없는 읽기 전용 메서드에 사용, 영속성 컨텍스트를 플러시 하지 않으므로 약간의 성능 향상(읽기 전용에는 다 적용)
  - 데이터베이스 드라이버가 지원하면 DB에서 성능 향상
- `Autowired` : 생성자 Injection 많이 사용, 생성자가 하나면 생략 가능
  - 필드 주입 대신 생성자 주입을 사용하자.
  - 변경 불가능한 안전한 객체 생성이 가능하다.
  - `final`키워드를 추가하면 컴파일 시점에 `memberRepository`를 설정하지 않는 오류를 체크할 수 있다.
```java
// 필드 주입
public class MemberService {

  @Autowired
  MemberRepository memberRepository;
}


// 생성자 주입
public class MemberService {
  private final MemberRepository memberRepository;
  
  public MemberService(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
  }
}


// lombok
@RequiredArgsConstructor
public Class MemberService {
  private final MemberRepository memberRepository;
}
```
- `@RunWith(SpringRunner.class)` : 스프링과 테스트 통합
- `@SpringBootTest` : 스프링부트 띄우고 테스트(이게 없으면 `@Autowired` 다 실패)
- `@Transactional` : 반복 가능한 테스트 지원, 각각의 테스트를 실행할 때마다 트랜잭션을 시작하고, 테스트가 끝나면 트랜잭션을 강제로 롤백(이 어노테이션이 테스트케이스에서 사용될 때만 롤백)
- 도메인 모델 패턴 : 비즈니스 로직이 대부분 엔티티에 있고, 서비스 계층은 단순히 엔티티에 필요한 요청을 위임하는 역할을 하는 것
- 트랜잭션 스크립트 패턴 : 엔티티에는 비즈니스 로직이 거의 없고 서비스 계층에서 대부분의 비즈니스 로직을 처리하는 것

### 폼 객체 vs 엔티티 직접 사용
- 요구사항이 정말 단순할 때는 폼 객체 없이 엔티티를 직접 등록과 수정 화면에서 사용해도 된다.
- 그러나 화면 요구사항이 복잡해지기 시작하면, 엔티티에 화면을 처리하기 위한 기능이 점점 증가한다. 결과적으로 엔티티는 점점 화면에 종속적으로 변하고, 이렇게 화면 기능 때문에 지저분해진 엔티티는 결국 유지보수하기 어려워진다.
- 실무에서 엔티티는 핵심 비즈니스 로직만 가지고 있고, 화면을 위한 로직은 없어야 한다. 
- 화면이나 API에 맞는 폼 객체나 DTO를 사용해라. 그래서 화면이나 API 요구사항을 이것들로 처리하고, 엔티티는 최대한 순수하게 유지해야 한다.


## 변경 감지와 병합(merge) <중요>
### 준영속 엔티티
- 영속성 컨텍스트가 더는 관리하지 않는 엔티티
  - 코드에서 `itemService.saveItem(Book)`에서 수정을 시도하는 Book 객체.
  - Book 객체는 이미 DB에 저장되어 있어 식별자가 존재하는 상태이다.
  - 임의로 만든 엔티티도 기존 식별자를 가지고 있으면 준영속 엔티티로 볼 수 있다.

### 준영속 엔티티를 수정하는 2가지 방법
- 변경 감지 기능
- 병합(merge)

#### 변경 감지 기능
```java
@Transactional
void update(Item itemParam) { // itemParam : 파라미터로 넘어온 준영속 상태의 엔티티
  Item findItem = em.find(Item.class, itemParam.getId()); // 같은 엔티티를 조회한다.
  findItem.setPrice(itemParam.getPrice()); // 데이터를 수정한다.
}
```
- 영속성 컨텍스트에서 엔티티를 다시 조회한 후에 데이터를 수정하는 방법
- 트랜잭션 안에서 엔티티를 다시 조회, 변경할 값 선택하면, 트랜잭션 커밋 시점에 변경 감지(Dirty Checking)이 동작해서 데이터베이스에 UPDATE SQL을 실행한다.

#### 병합
```java
@Transactional
void update(Item itemParam) { // itemParam : 파라미터로 넘어온 준영속 상태의 엔티티
  Item mergeItem = em.merge(item);
}
```
- 준영속 상태의 엔티티를 영속 상태로 변경할 때 사용하는 기능이다.
- 동작 방식
  - 1. `merge()`를 실행한다.
  - 2. 파라미터로 넘어온 준영속 엔티티의 식별자 값으로 1차 캐시에서 엔티티를 조회한다.
    - 2-1. 만약 1차 캐시에 엔티티가 없으면 데이터베이스에서 엔티티를 조회하고, 1차 캐시에 저장한다.
  - 3. 조회한 영속 엔티티에 엔티티의 값을 채워 넣는다.
  - 4. 변경된 영속 상태인 엔티티를 반환한다.
- 간단 정리
  - 1. 준영속 엔티티의 식별자 값으로 영속 엔티티를 조회한다.
  - 2. 영속 엔티티의 값을 준영속 엔티티의 값으로 모두 교체한다.(병합한다.)
  - 3. 트랜잭션 커밋 시점에 변경 감지 기능이 동작해서 데이터베이스에 UPDATE SQL이 실행된다.
- 변경 감지 기능을 사용하면 원하는 속성만 선택해서 변경할 수 있지만, 병합을 사용하면 모든 속성이 변경된다. 병합시 값이 없으면 null로 업데이트할 위험도 있다. (병합은 모든 필드를 교체한다.)

### 새로운 엔티티 저장과 준영속 엔티티 병합을 편리하게 한번에 처리
```
@Repository
public class ItemRepository {

  @PersistenceContext
  EntityManager em;

  public void save(Item item) {
    if (item.getId() == null) {
      em.persist(item);
    } else {
      em.merge(item);
    }
  }
}
```
- 위에서 `save()`메서드는 저장과 수정(병합)을 모두 처리한다.
- 신규 데이터 저장 뿐만 아니라 변경되 데이터의 저장도 가능하다.
- 이렇게 함으로써 이 메서드를 사용하는 클라이언트는 저장과 수정을 구분하지 않아도 되므로 클라이언트의 로직이 단순해진다.
- 영속 상태의 엔티티는 변경 감지(dirty checking)기능이 동작해서 트랜잭션을 커밋할 때 자동으로 수정되므로 별도의 수정 메서드를 호출할 필요가 없고 그런 메서드도 없다.
- 실무에서는 병합을 추천하지 않는다. 병합은 모든 데이터를 변경하고, 데이터가 없으면 Null로 업데이트 하므로, 항상 변경 폼 화면에서 모든 데이터를 유지해야 하는 문제가 있다.

### 해결방법
- 엔티티를 변경할 때는 항상 변경 감지를 사용하자.
- 컨트롤러에서 어설프게 엔티티 생성하지 말자.
- 트랜잭션이 있는 서비스 계층에 식별자와 변경할 데이터를 명확하게 전달하자. (파라미터 or DTO)
- 트랜잭션이 있는 서비스 계층에서 영속 상태의 엔티티를 조회하고, 엔티티의 데이터를 직접 변경하자.








