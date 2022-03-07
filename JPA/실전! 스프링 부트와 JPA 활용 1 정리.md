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


