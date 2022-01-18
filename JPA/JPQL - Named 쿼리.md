# JPQL - Named 쿼리
> 김영한님의 [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard) 강의 내용 정리입니다.

#### 정적 쿼리
- 미리 정의해서 이름을 부여해두고 사용하는 JPQL
- 정적 쿼리
- 어노테이션이나 XML에 정의할 수 있음 (어노테이션이 더 쉬움)
- 애플리케이션 로딩 시점에 초기화 후 재사용
- (중요) 애플리케이션 로딩 시점에 쿼리를 검증
  - SQL의 오류를 사전에 방지 

```java
@Entity
@NamedQuery(
        name = "Member.findByUsername",
        query = "select m from Member m where m.username = :username"
)
public class Member { ... }


// JPA 실행 클래스에서
List<Member> resultList24 = em.createNamedQuery("Member.findByUsername", Member.class)
                    .setParameter("username", "member1")
                    .getResultList();
```

#### Named 쿼리 환경에 따른 설정
- XML이 항상 우선권을 가진다.
- 애플리케이션 운영 환경에 따라 다른 XML을 배포할 수 있다.


#### 실무 조언
- 실무에서는 Spring Data JPA를 섞어 쓰는 것이 좋기 때문에, Repository 인터페이스에서 @Query를 사용하는 것을 권장
