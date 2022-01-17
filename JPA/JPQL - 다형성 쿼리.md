# JPQL - 다형성 쿼리
> 김영한님의 [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard) 강의 내용 정리입니다.

~~(크게 중요하지 않음)~~

#### TYPE
- 조회 대상을 특정 자식으로 한정 (ex. Item 중에 Book, Movie를 조회해라)
```
[JPQL]
select i from Item i where type(i) IN (Book, Movie)

[SQL]
SELECT i from ITEM i WHERE i.DTYPE in ('B', 'M')
```

#### TREAT (JPA 2.1 이상)
- 자바 타입 캐스팅과 유사
- 상속 구조에서 부모 타입을 특정 자식 타입으로 다룰 때 사용
- FROM, WHERE, SELECT(하이버네이트 지원) 사용
```
[JPQL]
select i from Item i where treat(i as Book).author = 'kim'

[SQL]
SELECT i from ITEM i WHERE i.DTYPE = 'B' AND i.author = 'kim'
```
