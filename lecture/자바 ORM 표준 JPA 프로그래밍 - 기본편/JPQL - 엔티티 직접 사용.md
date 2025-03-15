# JPQL - 엔티티 직접 사용
> 김영한님의 [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard) 강의 내용 정리입니다.

#### 기본 키 값
- JPQL에서 엔티티를 직접 사용하면 SQL에서 해당 엔티티의 기본 키 값을 사용
```java
[JPQL]
select count(m.id) from Member m    // 엔티티의 아이디를 사용
select count(m) from Member m       // 엔티티를 직접 사용

[SQL]
select count(m.id) as cnt from Member m
```
- 엔티티(Member)를 파라미터로 전달할 수 있다. 이때 식별자(memberId)를 직접 전달하는 것과 똑같은 SQL이 나간다.
```java
// 엔티티를 파라미터로 전달
String jpql = "select m from Member m where m = :member";
List result = em.createQuery(jpql).setParameter("member", member).getResultList();

// 식별자를 파라미터로 전달
String jpql = "select m from Member m where m.id = :memberId";
List resultList = em.createQuery(jpql).setParameter("memberId", memberId).getResultList();

// 실행된 SQL
select m.* from Member m where m.id=?
```

#### 외래 키 값
```java
Team team = em.find(Team.class, 1L);

// 엔티티를 파라미터로 전달
String jpql = "select m from Member m where m.team = :team";
List resultList = em.createQuery(jpql).setParameter("team", team).getResultList();

// 식별자를 파라미터로 전달
String jpql = "select m from Member m where m.team.id = :teamId";
List resultList = em.createQuery(jpql).setParameter("teamId", teamId).getResultList();

// 실행된 SQL
select m.* from Member m where m.team_id=?
```
