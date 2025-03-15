# Learned
## Transaction 관련
### Transaction의 격리 수준 (Isolation Level) - Read Uncommited
- 스프링 프레임워크에서 제공하는 Transaction의 격리 수준 기본값은 Read Commited이다.
  - 즉, commit되지 않은 트랜잭션은 다른 트랜잭션에서 읽을 수 없다.
  - 동시성에서 문제가 발생하기 쉽다. 특히, 진행 중인 트랜잭션의 내용을 무시하고 데이터를 읽어 처리하기 때문에, 동시에 다른 트랜잭션에서 먼저 처리 후 DB에 커밋해버리면, 의도치 않은 문제가 생긴다.
- 다른 트랜잭션에서 일어나는 데이터의 변화를 읽으려면 격리 수준을 Read Uncommited를 사용한다.
  - 그러나 Dirty Read 문제라는 단점이 존재한다.

### Exception 없이 Transaction 롤백 시키기
- `TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();` 를 사용하면, Exception 예외를 던지지 않고도 롤백을 강제할 수 있다.
- `TransactionAspectSupport`는 스프링 프레임워크에서 지원하는 트랜잭션 관련 클래스이다.
  - 특히, `currentTransactionStatus()`를 활용해 현재 트랜잭션에 대한 정보 얻기와 속성 설정을 할 수 있다.
    - rollbackOnly 여부, transaction 이름, readOnly 여부, flush 등 지원

### Multi DataSource 운영 시 주의할 점!
- 기본적으로 다른 DB 끼리 트랜잭션이 합쳐지지 않는다. 이를 합치려면 ChainedTransaction을 구현해야 한다.
  - 그러나 이 방법이 좋은 것만은 아닌 것이, 특정 조건에서는 롤백이 제대로 이뤄지지 않을 수 있다고 한다.
- 패키지 기준으로 멀티 DB 설정을 해주어도, `@Transactional` 어노테이션은 기본 PlatformTransactionManager을 사용도록 되어있다.
- 그러니까, DB 설정을 하면서 `@Primary`로 설정한 TransactionManager가 기본값으로 들어간다.
- JPA를 쓰면서 가장 문제가 되는 부분은, `@Primary`로 설정하지 않은 패키지에서 @Transactional을 달아버리면 트랜잭션 매니저가 다르기 때문에 Dirty-Checking이 안 된다!
- 주 DB가 아닌 서브 DB를 사용하는 패키지에서는 반드시 transactionManager를 명시해줘야 한다. (@Transactional의 기본 파라미터가 트랜잭션 매니저이므로, 해당 이름만 써주면 된다.)

### 다중 TransactionManger
- DB가 여러 개가 붙을 경우, 보통 각 DB마다 TransactionMange를 만들게 된다.
  - (JPA) DataSource를 정의하고 EntityMangerFactory에 넣고 TransactionManger를 만든다.
- 여러 개의 TransactionManager가 만들어지면, 그 각각의 Transaction에 대해서는 원자성이 보장된다. 그러나 여러개의 Transaction이 한 군데로 모이면, 원자성이 보장되지 않는다. 
  - 어느 한 쪽의 Transaction이 예외가 발생하여 롤백이 된다 하더라도, 다른 Transaction은 영향을 받지 않아 그대로 commit되어 버린다.
- 이를 방지하기 위한 기술로 Spring이 지원했던 것이 ChainedTransactionManager이다. (_Deprecated_)
  - 여러 트랜잭션매니저에 순서를 부여하여 ChainedTransactionManager를 정의한다.
  - ChainedTransactionManager에 등록된 순서대로 Transaction이 시작되며, commit은 그 역순이 된다. (즉, 가장 처음 등록된 트랜잭션 매니저의 트랜잭션이 가장 마지막에 커밋된다.)
  - 문제는, 상황에 따라 원자성이 보장되지 않는다는 점이다.
    - 만약 트랜잭션 매니저가 A,B,C 총 3개이고, A,B,C 순서로 등록되었다고 하자. 
    - 커밋 순서는 C,B,A가 된다.
    - C에서 롤백이 일어나면, B,A는 커밋이 되지 않는다.
    - B에서 롤백이 일어나면, C는 커밋이 되고, A는 커밋되지 않는다.
    - A에서 롤백이 일어나면, B,C 모두 커밋이 된다.
  - 따라서 ChainedTransactionManager를 사용할 때는, 예외 상황이 가장 일어나지 않는 것을 처음에 등록한다.
  - 달리 말하면, 예외가 자주 일어나거나, 전체 롤백이 필요한 로직들을 가장 마지막에 등록하는 식으로 운영한다.
- 이러한 단점을 극복하기 위해 나온 기술들 중 하나가 AtomikosJta이다.
  - 다중 트랜잭션 매니저를 하나의 트랜잭션 매니저처럼 처리해주는 기술이다.
  - 여러 트랜잭션을 하나로 묶어 처리하는 것을 글로벌 트랜잭션이라고 한다.
  - ChainedTransactionManager와 다르게, 하나의 트랜잭션처럼 처리해준다.
  - JtaTransactionManager 관련 기술이라고 하는데, (Jpa가 아닌 Jta이다) 여기에 대해서는 아직 레퍼런스가 많지 않은 것 같다.

### 트랜잭션에 관하여
- `@Transactional`에 TxManager 명시 안 하면 JPA 기본 TxManager가 생성됨. (이름 없는 TxManager)
- `@Transactional`을 안 붙이고 JPA/MyBatis를 사용할 경우, 기본 TxManager로 트랜잭션이 각각 생성됨.
- JPA와 MyBatis를 동시에 쓰고자 한다면 반드시 호출하는 메서드에 같은 트랜잭션으로 묶어주어야 함.
  - 그렇지 않을 경우, 먼저 호출된 것만 롤백이 됨. 나머지는 롤백이 안 됨. 트랜잭션을 알아서 생성해버리기 때문.
  - ex) JPA 호출 -> MyBatis 호출 -> Exception : JPA만 롤백
  - ex) MyBatis 호출 -> JPA 호출 -> Exception : MyBatis만 롤백
- 멀티 DB에서 다른 영역의 TxManager 지정 시
  - `@Transactional`이 아무곳에도 걸려있지 않은 경우 : 다른 영역에서는 read-only로 트랜잭션을 생성
  - `@Transactional`로 TxManager를 지정하더라도, 다른 영역에서는 TxManager를 알아서 생성함. 실제로 CRUD가 가능하지만, 롤백은 안 됨.
  - ChainedTransactionManager를 사용하면 어느 정도 해결 가능하지만, Deprecated되었음.
  - 최근에는 JtaTransactionManager를 활용
    - 특히 atomikos 라이브러리를 활용하여 여러 DB소스 트랜잭션을 마치 하나의 트랜잭션처럼 묶어서 처리해줌 (글로벌 트랜잭션 처리)


## Kotlin 관련
### Kotlin 토막 지식
- java보다 클래스 타입이 훨씬 많다. object, class, interface, sealed interface, data class, enum class 등등...
- java에서 자주 사용하는 타입들을 편의상 만들어준 모양새다. (data class는 Lombok의 `@Data`를 달아놓은 클래스와 거의 같다.)
- object로 선언된 클래스의 인스턴스들은 모두 싱글턴으로 관리된다.
- enum class로 선언된 것들은 compile-time constant가 아니다. 그 이후 시점에서 constant가 되는 것 같다.
  - 알게 된 이유는, enum을 어노테이션의 속성 값으로 넣으려고 하면 코틀린에서 컴파일 에러를 낸다. 그래서 대신에 object를 하나 생성하고 const val로 처리했다.
- `inline` 함수
  - 람다를 전달할 때 생기는 메모리 등의 오버헤드를 해소하기 위한 키워드.
  - 람다의 구현 형태를 미리 정해놓는 것이라고 생각하면 쉽다.
  - 타입에 대한 정보를 런타임에서도 받을 수 있도록 reified 라는 키워드를 활용할 수 있다.

### Kotlin과 `@ConfigurationProperties`
- `@ConfigurationProperties`는 properties나 yaml 파일의 접두어를 지정해 값을 읽어오는 어노테이션이다.
- Kotlin에서 활용할 때는 가져오고자 하는 값과 똑같은 이름의 프로퍼티를 선언한 후에 `lateinit`을 사용한다.
- 해당 프로퍼티는 private이나 internal 한정자를 사용할 수 없다. public(기본값)을 사용해야 한다.
- 복습) Int나 Boolean 같은 원시 타입은 lateinit 사용이 안 된다.

### Kotlin : List에 관하여
- Kotlin의 List는 기본으로 불변(immutable) 속성을 갖는다.
  - 따라서 Java처럼 List에 add가 안 된다. 애초에 메서드가 없다.
  - 대신 plus가 있는데, 해당 List 인스턴스를 복사해 새로운 element를 추가하여 새 List를 생성하여 반환한다.
- 따라서 Java의 List를 Kotlin에서 사용하고 싶다면, MutableList를 사용한다.
  - 우리가 원하는 add 메서드가 있다.
  - 초기화 하는 방법
```Koltin
// 방법 1
val testList1 = mutableListOf<Any>()

// 방법 2
val testList2: MutableList<Any> = mutableListOf()
```
- awaitAll() 로 리스트 안의 작업들을 한꺼번에 실행시킬 수 있다.
  - `<T> Collection<Deferred<T>>.awaitAll()`
  - 원소를 순차적으로 탐색하며 호출하는 await과는 조금 다르다.
  - awaitAll은 하나라도 실패할 Deferred가 발생할 경우 모든 작업을 한꺼번에 중지한다. 반면, 순차탐색으로 호출한 await은 실패하는 Deferred가 나올 때까지 계속 탐색한다.
- flatten() 예시
  - List 안에 List 들이 있을 경우, 이를 단일 List으로 변환해준다.
```Kotlin
// examList의 형태가 [[1, 2, 3], [4, 5], [6]]이라면,
val flattenList: List<Int> = examList.flatten()
println(flattenList) // [1, 2, 3, 4, 5, 6]
```
- List에 메서드를 담고 싶으면, `List<(paramTypes) -> (resultType)>`로 선언할 수 있다.
  - 다만 파라미터 타입과 반환 타입이 같은 메서드들만 할 수 있다.

### Coroutine in Kotlin
- `runBlocking`을 통해 블러킹을 구현할 수 있다. 블록 안의 작업이 끝날 때까지 스레드를 잠시 멈출(suspend) 수 있다.
  - 블록 안에서 비동기 처리가 가능하다.
    - 결과값 반환이 필요없으면 `launch`
    - 결과값 반환이 필요하면 `async`
  - `async`는 반환 타입이 `Deferred`이고, `await` 메서드를 통해 결과를 반환 받을 수 있다.

### Gradle with Kotlin DSL
- 코틀린이든 자바든 Spring에서 Gradle의 SourceSets 의 기본값은 java이다.
- 검색 결과도 그러하던데, 코틀린파일(.kt)는 컴파일 과정에서 제외할 수 없다. 공식 스펙은 아닌 것 같고, 사람들의 아우성 뿐이다.
- 컴파일은 기본적으로 정해진 파일 양식 이외에는 컴파일을 안한다. 그러니까, 마이바티스 Mapper XML 파일을 src/main/kotlin(혹은 java)에 넣어도 빌드하면 xml 파일은 자동으로 제외된다.
  - 이를 해결하려면 resources 파일에 classes 에 해당하는 것들을 다시 넣던지, 아니면.. 어떻게서든지 xml 파일을 컴파일에 포함시키던지 해야하는데, Gradle이 말을 안 듣는다. 관련 명령어 조합을 수십개는 해본 것 같은데, resources에 소스파일 일부가 중복해서 들어가는 것 말고는 모두 실패였다. 

## Untitled
### 코드 포맷팅, 블록 Depth 줄이기
- if 문 중, '특정 조건은 아래의 코드를 실행하지 않음'의 탈출 기능을 한다면, 그것을 최상단으로 올리고 `if(...) return;` 형태로 코딩하여 아래 코드들의 Depth를 줄이기.
- 변수를 사용하는 기준
  - 1. 이름을 통한 기능 명시
  - 2. 재사용성
  - 접근하는 메서드를 통해 가져오는 변수가 이미 어떤 기능인지 이해할 수 있고, 한번 밖에 사용하지 않는 변수는 따로 변수할당을 하지않고 하드코딩 혹은 getter 형식의 접근자를 그대로 사용하였음.

### 코딩 관련
- 기능에 초점을 맞춘다면 반복문을 나누는 것도 나쁘지 않은 선택일 수 있다. 과하지만 않다면.
- 절차 지향적 프로그래밍에서는 하나의 변수가 어디서 변화를 받는지 쉽게 파악하기 어렵다. 선언형(함수형) 프로그래밍이 나온 이유를 알 것 같다.
- 적어도 변수를 기능단위로 쪼개고, 될 수 있으면 메서드 파라미터는 불변하도록 처리하는 것이 유지보수 측면에서도 좋은 것 같다.
- INSERT는 될 수 있으면 Bulk Query로 처리하기.
- 코딩을 하면 할수록 과유불급이란 말이 떠오른다. 지나치게 꼼꼼한 것도, 지나치게 추상화하는 것도 좋지 못 하다.

### 업무 관리에 대하여
- 일을 할 때는, 목표를 분명히 한다.
- 그리고 그날 그날 작은 목표를 세우고, 공유한다.
- 개발의 경우, 로컬 테스트 환경을 통일시킬 필요가 있으므로, 내용을 적어서 ReadMe.md 같은 파일로 만들어 공유하자.
- 함께 일을 하게 될 경우, 나눌 수 있는 일은 나누도록 하고, 누가 어떤 일을 하는지를 명확히 하는 것이 좋다.
- 스크럼 같은 자리에서 하는 일을 잘 공유하도록 하고, 고민하다가 막힌다 싶으면 동료들에게 도움을 청하자.
  - 특히 나같은 경우 너무 혼자하려고만 하는 건 아닌지 되돌아보자. 특히 재택이면 더더욱 그렇다.

### `CompletableFuture.runAsync()`
- `runAsync()`의 `Runnable`을 구현하는 블록에서는 Checked Exception을 던질 수 없다.
- `runAsync()`의 반환 타입은 `CompletableFuture<Void>`이며 `exceptionally()`로 `Throwable`을 처리할 수 있다.
- `runAsync()` 내에서의 Checked Exception은 try-catch 블록으로 감싸줘야 컴파일이 된다.

### CompleatableFuture
- 비동기 처리할 만큼 `runAsync `혹은 `supplyAsync` 사용
- 비동기 처리할 메서드를 `List<Supplier<?>>` 혹은 `List<Consumer<?>>`로 리스트에 담고 `forEach` 사용하여 스트림으로 연결하기
- `supplyAsync` 혹은 `runAsync` 영역에서 발생한 CheckedException은 블록 바깥으로 전파되는 것을 허용하지 않음. CheckedException은 반드시 try~catch 구문으로 처리해야 함.
  - UncheckedException(`RuntimeException `하위)의 경우 발생하여도 다른 스레드에 전파되지 않음. 즉 다른 블록(스레드)의 작업에 영향을 주지 않음.
  - 다만 로그에 그 stack trace가 찍히지 않기 때문에, logging으로 Exception을 잘 끌고 올 것.
- `runAsync` 혹은 `supplyAsync`에 연결하는 메서드인 `handle`과 `exceptionally`는 주로 UncheckedException을 처리함.
- `runAsync` 혹은 `supplyAsync` 블록이 예외를 던짐으로 끝내고 싶다면 `completeExceptionally`를 사용
- `supplyAsync`에서 나온 결과값을 가지고 다른 값을 만들고 싶으면 `thenApply`를, void로 끝내고 싶으면 `thenAccept`를 사용

### `@Slf4j`와 스택 트레이스
- log 인스턴스(`Log4jLogger` 등)으로도 Exception이 터졌을 때의 스택 트레이스를 찍을 수 있다.
- 흔히 쓰는 `e.printStackTrace()`의 경우, `System.err`을 사용하기 때문에 스택 트레이스의 비용이 비교적 크다.
  - `Throwable` 클래스에서 확인 가능, `System.err`은 `PrintStream` 객체
- info(), warn(), error(), debug(), trace() 등의 메서드들은 모두 많은 다중정의가 되어 있는데, 그중 파라미터가 `(String var1, Throwable var2)` 형태인 메서드를 통해 스택 트레이스를 활용할 수 있다.
  - 예를 들면, `log.error("에러가 있음", e)` 혹은 `log.error(e.getMessage(), e)` 같은 형식으로 임의의 문자열과 스택 트레이스를 함께 찍을 수 있다.
  - 메서드 시그니쳐에 유의하자. 파라미터 타입이 `(String var1, Throwable var2)`인 메서드만 스택 트레이스를 활용할 수 있다.
  - 실무에서는 이 방법이 가장 깔끔한 듯하다.
- 로그를 찍을 때는
  - 1) 예상되는 Exception과 관련 깊은 parameter 로그
  - 2) 스택 트레이스를 위한 로그
  - 위의 2 쌍을 기본으로 사용하려고 함.

### 인터페이스, API, HTTP API, REST API
- [인터페이스](https://ko.wikipedia.org/wiki/%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4_(%EC%BB%B4%ED%93%A8%ED%8C%85))
  - 서로 다른 두 개의 시스템, 장치 사이에서 정보와 신호 교환을 하는 경계를 뜻한다.
  - 사용자가 기기를 쉽게 동작시키는데 도움을 주는 시스템을 뜻한다.
- [API (Application Programming Interface)](https://ko.wikipedia.org/wiki/API)
  - 소프트웨어의 인터페이스.
  - 컴퓨터나 컴퓨터 프로그램 사이의 연결이다.
  - 다른 종류의 소프트웨어에 서비스를 제공한다.
  - 시스템이 동작하는 방식에 관한 내부의 세세한 부분을 숨기는 것이 목적 중 하나이다.
- [HTTP API](https://ko.wikipedia.org/wiki/HTTP)
  - HTTP를 사용해 서로 정해진 스펙으로 데이터를 주고 받으며 하는 통신을 뜻한다.
- [REST API](https://ko.wikipedia.org/wiki/REST)
  - HTTP API의 장점을 최대한 활용하는 방법이다.
  - HTTP API에서 4가지 제약조건을 만족해야 한다.
    - 자원의 식별
    - 메시지를 통한 리소스 조작
    - 자기서술적 메시지
    - 애플리케이션의 상태에 대한 엔지으로서 하이퍼미디어 (HATEOAS)
  - 위의 조건을 최대한 지키면서 개발하는 것을 RESTful API라고 하는데, 특히 4번째 제약조건이 구현에 어려움이 있다. 개발 비용 대비 효과가 크지도 않다.
  - 엄격한 기준에서는 HTTP API와 다르나, 보통 거의 같은 의미로 사용된다.

### IntelliJ 단축키
- `cmd + 좌클릭`으로 다른 곳으로 넘어갔다가 다시 되돌아올 때 쓰는 단축키는 `cmd + [`

### `$.ajax`의 success, error, complete
- success(Anything data, String textStatus, jqXHR jqXHR) : 통신 성공했을 때 콜백
- error(jqXHR jqXHR, String textStatus, String errorThrown) : 통신이 실패했을 때 콜백
- complete(jqXHR jqXHR, String textStatus) : try-catch-finally의 finally의 느낌으로, error나 success 이후 처리하는 콜백

### jqXHR 객체?
- 브라우저의 XMLHttpRequest를 대체하는 객체.
- [jQuery 공식 사이트](https://api.jquery.com/jQuery.ajax/#jQuery-ajax-settings-settings)에서 여러가지 속성을 가짐을 알 수 있음.
- 객체를 통해 ResponseHeader, responseText, responseJSON, status(HTTP 오류 코드), statusText(HTTP 오류 메시지) 등을 편리하게 접근할 수 있음.
- $.ajax는 이 jqXHR을 반환하는데, 이 특징을 활용해 ajax를 전체를 감싸 비슷한 역할을 하는 done, fail, always 메서드를 사용해 콜백을 다시 한 번 설정할 수 있음.

### `@ExceptionHanlder` 설계에서 실패하더라도 필요하다면 데이터를 넘겨줄 수 있도록 설계하자.
- 데이터를 넘겨줄 수 없으니, ajax error(혹은 fail) 콜백 함수에서 원하는 처리가 불가능해 success(혹은 done)에서 처리하게 된다. RESTful하지 않은 설계는 유지보수에 혼란을 준다.

### Spring Cloud
- MSA 트렌드에 맞게 나온 스프링 프로젝트.
- 단순히 웹 애플리케이션 서버에 대한 것뿐 아니라, Spring Cloud 만으로도 서버 하나를 띄울 수 있게 해준다. (로드밸런싱, 라우터 등등 까지 포함)

### DB property 설정 꿀팁
- `AvailableSettings` 이란 인터페이스가 있다. 여기에 hibernate 설정을 싹다 모아놨다. 만드신 Steve Ebersole님 상줘야 한다.

### Swagger로 깨달은 버전 간 호환성 주의하기
- SpringBoot 2.7.3 + Kotlin은 springfox가 제공하는 swagger가 작동을 안 한다. 
  - baseUrl을 추론할 수 없다, nullPointerException 갖가지 이유로 어떤 버전을 바꾸든 안된다.
- 다행히 SpringDoc에서 제공하는 dependency를 통해 해결하였다.
- 버전 간 충돌이 있음을 주의하자. 무조건 최신이라고 좋은 것만이 아니다. 버그가 있을 수도 있고, 기존 코드가 작동하지 않을 수도 있다.
- 특히 Kotlin + Spring은 레퍼런스가 그렇게 풍부한 것은 아닌 것 같다.

### 패키지 구조 - 정해진 것은 없다.
- 패키지 구조는 공부할수록 정해진 답이 없는 것 같다.
- MSA의 트렌드로 기능을 분리할 수 있도록 패키지를 구성하는 것 또한 트렌드인 것 같다.
  - 규모가 작을 때는 실용적으로 접근해서 한 패키지에 몰아넣는 것도 좋은 방법이다.
  - 규모가 커지고, 도메인이 서로 복잡하게 얽히기 시작하면, 계층형도 나쁘지 않은 방법인 듯 하다.
  - 언제나 규모가 커졌을 때가 문제다. 적절히 분리해야 할 것 같다.

### Spring Cloud OpenFeign
- Netflix에서 개발한, HTTP API 통신을 위한 라이브러리이다.
- JPA처럼 인터페이스에 간단히 몇가지 선언만 해주면 HTTP API Request가 가능하다. ([참고자료](https://techblog.woowahan.com/2630/))

### Docker + Actuator, Prometheus, Grafana 연결하기
- 생각보다 쉽다. 도커에 이미지 받고, 컨테이너 설정하면 끝.
- Actuator를 통해 지난 httptrace 등 정보를 볼 수 있다. 최신 100개라고 한다.
- 모니터링이 된 것을 저장하는 건 어떻게 하는 것인지 찾아보아야 겠다. 특히 Actuator httptrace는 좀 탐난다.

### Kafka
- Kafka 클러스터 + zookeeper가 있어야 kafka를 실행할 수 있다.
- producer에서 메시지(Topic)를 생성하고 consumer로 보내면 데이터가 쌓인다.
- HTTP API보다 효율적이다. 비동기를 안정적으로 처리할 수 있다.
- kafka을 매개로하여 서버끼리 통신을 가능케 한다.  

### RedisCacheManager
- Spring Data Redis는 Spring Data JPA 처럼 인터페이스(CrudRepository)를 상속받아 선언하면 알아서 구현체를 만들어준다.
- 트랜잭션 기능을 사용하려면 RedisTemplate을 구현하여 사용한다.
- Redis는 DB와 크게 다르지 않다. 어렵게 생각하지 말자.
- EHCache와 같이 어노테이션을 사용해 캐싱이 가능하다.
  - 차이점이라면 EHCache는 서버가 종료되면 캐싱이 삭제되지만, Redis는 그렇지 않다. (로컬 캐싱 vs 글로벌 캐싱)
  - @Cacheable을 통해 캐시 쓰기&읽기, @CachePut을 통해 캐시 쓰기를, @CacheEvict를 통해 캐시 삭제가 가능하다.
  - id는 @Cacheable을 선언한 메서드의 파라미터를 활용할 수 있다. # 문자를 통해 접근 가능하다.

### JS, EventListener 사용하기
- EventListener는 AddEventListener과 RemoveEventListener로 등록/해제할 수 있다.
- 옵션이 많다. 공식 문서 참고
- 함수를 선언하고 등록/해제 메서드에 Arguments로 넘겨주면 작동한다.
- 적용된 뷰를 보려면 캐시 무력화 항상 생각하기


# Questions
- [ ] `CompletableFuture.runAsync()` 내에서 Checked Exception은 `handle()`, `exceptionally()` 등으로 처리가 안 된다. 어떻게 처리해야 깔끔한 코딩이 될까?
  - try-catch 대신에 메서드 체인 형식의 함수형 프로그래밍으로 대처 불가능한가?
- [ ] HTTP API 통신을 위한 OkHttpClient 기반 Retrofit2 클래스를 스프링 빈으로 등록해서 관리하기 vs 사용할 때마다 new로 생성하기
  - 해당 클래스를 빈 등록 후 싱글톤으로 사용했을 때 문제점이 있는가? (예를 들면, 동시성 문제, 쓰레드 관리 문제 등)
- [x] 트랜잭션 롤백을 실행시키려면 Exception을 던지는 것 외에 다른 방법이 없을까?
  - #3 , `TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();` 활용하기


# 고민
- [ ] try-catch에서 catch의 타입 한정짓기 vs `Exception`으로 사실상 모두 커버하기
  - 현재 작업 중인 레거시 코드 구조 상, 트랜잭션이 하나로 걸려있고(Propagation.REQUIRED 일괄 적용) 관리가 잘 되지않아 Exception이 다른 메서드로 전파될 경우 해당 트랜잭션이 모두 롤백되는 구조. 따라서 주로 방어적인 try-catch (`catch (Exception e)`)를 사용 중임.
  - 트랜잭션을 메서드마다 활용에 맞게 분리하는 작업이 필요 (트랜잭션 Propagation 및 Isolation 레벨에 대한 공부 필요)
  - 반복되는 try-catch를 지양하기 위해 커스텀 Exception은 RuntimeException을 상속하라는 팁의 유효성을 느낌
- [ ] try 블록 크기 관련
  - 최소화 -> 코드 분기가 발생 (가독성 저하 우려)
  - 하나로 묶을 수록, try 블록과 catch에서 담당해야 할 기능/책임/Exception이 많아짐, catch에서 찍어주는 log의 정보가 구체적인 것과 멀어지고 점점 일반화됨.
  - 담당 개발자의 선택의 문제인 것 같지만, 유지보수/가독성/확장성을 최대한 동시에 만족하는 좋은 코드를 만들고 싶음.
- [ ] `@ControllerAdvice`에서 Exception을 공통으로 처리하고 있어 임의의 data를 jsp 쪽으로 넘기기가 어려울 땐 어떻게 해야 할까?
  - 필요한 부분을 최상단으로 끌어올려 검증을 처리한다?
  - 검증에 실패했을 때 throw new Exception 형태가 아닌 return을 하면 HTTP 코드는 200으로 나온다. 에러지만 HTTP 통신에서는 에러라고 나오지 않는, RESTful 스럽지 않은 API가 된다.
- [ ] DELETE-INSERT 로 데이터를 보정하는 구조가 바람직하지 않다고 한다. 특정 시점의 저량 데이터를 나타내는데 좋은 DB 설계는 무엇인가.
- [x] ChainedTransaction 공부하기
- [ ] MyBatis XML을 어떻게 하면 Mapper 인터페이스와 함께 둘 수 있을지 고민하기. 
  - (빌드 후 resources에 kotlin파일을 한 번 더 포함한다 vs 빌드 전후 파일 구성을 똑같게 만들어 준다.)
