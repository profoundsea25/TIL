# FETCH JOIN에 관하여
> 김영한님의 [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard) 강의 내용 정리입니다.

#### 페치 조인(Fetch join) [실무에서 자주 사용, 정말 중요]
- SQL JOIN의 종류는 아니다.
- JPQL에서 성능 최적화를 위해 제공하는 기능이다.
- 연관된 엔티티나 컬렉션을 SQL 한 번에 함께 조회하는 기능
- join fetch 명령어 사용
- fetch join ::= [ LEFT [OUTER] | INNER ] JOIN FETCH 경로


#### JPQL의 DISTINCT의 2가지 기능
- SQL에 DISTINCT를 추가
- 애플리케이션에서 엔티티 중복 제거

#### 주의 : 1대다 JOIN이 일어나면 데이터가 '다' 쪽의 수만큼 데이터가 늘어나서 조회된다.

#### 페치 조인과 일반 조인의 차이
- 일반 조인 실행시 연관된 엔티티를 함께 조회하지 않음. 즉 결과를 반환할 때 연관관계를 고려하지 않음
- 페치 조인을 사용할 때만 연관된 엔티티도 함께 조회(즉시 로딩). 페치 조인은 객체 그래프를 SQL 한번에 조회하는 개념
-> 페치 조인으로 N+1 문제를 거의 해결한다.
