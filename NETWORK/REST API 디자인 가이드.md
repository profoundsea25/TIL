# REST API 디자인 가이드
> [REST API 제대로 알고 사용하기 (김동범, 2016-07-25)](https://meetup.toast.com/posts/92)를 요약한 내용입니다.

### 핵심 규칙 두 가지
- 첫 째, URI는 정보의 자원을 표현해야 한다.
- 둘 째, 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

#### 1) REST API 핵심 규칙 (예시)
- 리소스명은 동사보다는 명사를 사용한다.
```
# 회원 삭제 시
GET /members/delete/1    (X)
DELETE /members/1        (O)

# 회원 정보를 가져올 때
GET /members/show/1      (X)
GET /members/1           (O)

# 회원을 추가할 때
GET /members/insert/2    (X)
POST /members/2          (O)
```
#### 2) HTTP METHOD의 알맞은 역할
- POST : 리소스 생성
- GET : 리소스 조회 (해당 도큐먼트에 대한 자세한 정보 조회)
- PUT : 리소스 수정
- DELETE : 리소스 삭제
#### 3) URI 설계 시 주의할 점
- 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용
```
https://restapi.example.com/houses/apartments
https://restapi.example.com/animals/mammals/whales
```
- URI 마지막 문자로 슬래시(/)를 포함하지 않는다.
  - URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 함. URI가 다르다는 것은 리소스가 다르다는 것이고, 역으로 리소스가 다르면 URI도 달라져야 함. REST API는 분명한 URI를만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 URI 경로의 마지막에는 슬래시(/)를 사용하지 않음
```
https://restapi.example.com/houses/apartments/    (X)
https://restapi.example.com/houses/apartments     (O)
```
- 하이픈(-)은 URI 가독성을 높이는데 사용 (URI가 불가피하게 길어질 경우 사용)
- 밑줄(_)은 URI에 사용하지 않음
- URI경로에는 소문자 권장 (대소문자에 따라 다른 리소스로 인식하기 때문)
  - 참고 : RFC3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정되어 있음
- 파일 확장자는 URI에 포함시키지 않음
  - Accept Header를 사용하기
```
GET /members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg
```
#### 4) 리소스 간의 관계를 표현하는 방법
```
소우 'has'의 관계를 표현할 때
GET /users/{userid}/devices

관계명이 애매하거나 구체적 표현이 필요할 때 (예 - '좋아하는' 디바이스 목록)
GET /users/{userid}/likes/devices
```
#### 5) 자원을 표현하는 Collection과 Document
- Document : 객체 혹은 단순히 문서로 이해해도 무방, __단수__ 로 표현
- Collection : 문서(Document)들의 집합 혹은 객체들의 집합, __복수__ 로 표현
- Document과 Collection 모두 리소스라고 표현할 수 있으며 URI에 표현됨
```
http://restapi.example.com/sports/soccer
sports라는 컬렉션과 soccer라는 도큐먼트로 URI 구성.

http://restapi.example.com/sports/players/13
sports, players 컬렉션과 soccer, 13라는 도큐먼트로 URI 구성.
```
