> 김영한님의 [모든 개발자를 위한 HTTP 웹 기본 지식](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard) 강의 요약입니다.

# HTTP 헤더2 - 캐시와 조건부 요청
## 캐시 기본 동작
### 캐시가 없을 때
- 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
- 인터넷 네트워크는 매우 느리고 비싸다.
- 브라우저 로딩 속도가 느리다.
- 느린 사용자 경험

### 캐시 적용
- 서버에서 헤더에 cache-control: max-age=n (n초 동안 캐시 적용)을 넣어서 응답을 내려주면, 클라이언트 브라우저 캐시에 응답 결과를 저장함. (n초 동안 유효)
- 다시 요청을 보낼 때 캐시 유효 시간을 검증해서, 유효하면 요청을 보내지 않고 브라우저 캐시에서 응답 결과를 가져옴.
- 캐시 덕분에 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다.
- 비싼 네트워크 사용량을 줄일 수 있다.
- 브라우저 로딩 속도가 매우  빠르다.
- 빠른 사용자 경험
- 요청을 보냈을때, 브라우저 캐시에서 캐시 유효 시간이 지났다면, 서버를 통해 데이터를 다시 조회하고, 캐시를 갱신한다.
- 이때 다시 네트워크 다운로드가 발생한다.


## 검증 헤더와 조건부 요청1
- 캐시 유효시간이 초과해서 서버에 다시 요청하면 다음 두 가지 상황이 나타난다.
  - 1. 서버에서 기존 데이터를 변경함
  - 2. 서버에서 기존 데이터를 변경하지 않음
- 캐시 만료 후에도 서버에서 데이터를 변경하지 않음
- 생각해보면 데이터를 전송하는 대신에 저장해 두었던 캐시를 재사용 할 수 있다.
- 단 클라이언트의 데이터와 서버의 데이터가 같다는 사실을 확인할 수 있는 방법이 필요

### 검증 헤더 추가
- 서버가 응답을 줄 때 데이터가 서버에서 마지막에 수정된 시간을 헤더에 추가 (`Last-Modified` 헤더)
- 응답 결과를 클라이언트에서 캐시에 저장, 캐시 유효 시간과 함께 데이터 최종 수정일을 기록
- 캐시 시간이 초과되면, 클라이언트에서 데이터 요청을 보낼 때 데이터 최종 수정일(`if-modified-since`)을 함께 보냄.
- 데이터 최종 수정일이 바뀌지 않았다면, 서버에서 응답을 내려줄 때 헤더에 `304 Not Modified`를 보내줌. 이때 **HTTP Body가 없음.**
- 브라우저 캐시에 이미 저장했던 데이터를 재사용, 헤더 데이터 갱신

### 정리
- 캐시 유효 기간이 초과해도, 서버의 데이터가 갱신되지 않으면 `304 Not Modified` + 헤더 메타 정보만 응답(HTTP Body는 없음)
- 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신
- 클라이언트는 캐시에 저장되어 있는 데이터 재활용
- 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드
- 매우 실용적


## 검증 헤더와 조건부 요청2
#### 검증 헤더 (Validator)
- 캐시 데이터와 서버 테이터가 같은지 검증하는 데이터
- Last-Modified, ETag
#### 조건부 요청 헤더
- 검증 헤더로 조건에 대한 분기
- `If-Modified-Since`: Last-Modified 사용
- `If-None-Match`: ETag 사용
- 데이터가 변경되지 않았으면 304 Not Modified를 응답으로 내리고, 헤더 데이터만 전송 (HTTP Body 미포함)
- 데이터가 변경이 되었으면, 200 OK를 응답으로 내리고, 모든 데이터를 전송 (HTTP Body 포함)

### Last-Modified, If-Modified-Since(If-Unmodified-Since) 단점
- 1초 미만 단위로 캐시 조정이 불가능
- 날짜 기반의 로직 사용
- 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우 (예 : A->B->A, 파일의 날짜는 갱신되지만 내용은 같은 경우)
- 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
  - 예) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고 싶은 경우

### ETag, If-None-Match(If-Match)
- ETag(Entity Tag)
- 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
- 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)
- 진짜 단순하게 ETag만 보내서 같으면 유지, 다르면 다시 받음
- 캐시 제어 로직을 서버에서 완전히 관리
- 클라이언트는 단순히 이 값을 서버에 제공 (클라이언트는 캐시 매커니즘을 모름)
  - 예) 애플리케이션 배포 주기에 맞추어 ETag 모두 갱신


## 캐시와 조건부 요청 헤더
### 캐시 제어 헤더
- Cache-Contorl: 캐시 제어
- Pragma: 캐시 제어(하위 호환)
- Expires: 캐시 유효 기간(하위 호환)
#### Cache-Control : 캐시 지시어(directives)
- 캐시 컨트롤로 다 할 수 있다.
- `Cache-Control: max-age`
  - 캐시 유효 시간, 초 단위
- `Cache-Control: no-cache`
  - 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
- `Cache-Control: no-store`
  - 데이터에 민감한 정보가 있으므로 저장하면 안 됨 (메모리에서 사용하고 최대한 빨리 삭제)
#### Pragma : 케시 제어(하위 호환)
- Pragma: no-cache
- HTTP 1.0 하위 호환
- 거의 사용하지 않음
#### Expires : 캐시 만료일 지정(하위 호환)
- expires: Mon, 01, Jan 1990 00:00:00 GMT
- 캐시 만료일을 정확한 날짜로 지정
- HTTP 1.0부터 사용
- 지금은 더 유연한 Cache-Control: max-age 권장
- Cache-Control: max-age와 함께 사용하면 expires는 무시


## 프록시 캐시
#### Cache-Control : 캐시 지시어(directives) - 기타
- `Cache-Control: public`
  - 응답이 public 캐시에 저장되어도 됨 -> 프록시 캐시 서버에서 사용
- `Cache-Control: private`
  - 응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(기본값)
- `Cache-Control: s-maxage`
  - 프록시 캐시에만 적용되는 max-age
- `Age: 60` (HTTP 헤더)
  - 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)


## 캐시 무효화
#### Cache-Control : 확실한 캐시 무효화 응답
- `Cache-Control: no-cache, no-store, must-revalidate`
  - 캐시가 되면 안 되는 페이지에 사용
- `Pragma: no-cache`
  - HTTP 1.0 하위 호환용
#### Cache-Control : 캐시 지시어(directives) - 확실한 캐시 무효화 응답
- `Cache-Control: must-revalidate`
  - 캐시 만료 후 최초 조회시 원(origin) 서버에 검증해야 함
  - 원 서버 접근 실패시 반드시 오류가 발생해야 함 - 504(Gateway Timeout)
  - must-revalidate는 캐시 유효 시간 내라면 캐시를 사용함
#### no-cache vs must-revalidate
- 프록시 캐시 서버에서 원 서버로 접근이 불가능 할 때, no-cache는 에러 대신 과거 데이터라도 보여줄 수 있지만, must-revalidate는 반드시 오류를 발생시킴
- 꼭 no-cache와 must-revalidate를 모두 써야 안전
