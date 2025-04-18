# [Kotlin in Action(2/e)](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=358099845)
- 지은이: 세바스티안 아이그너, 로만 엘리자로프, 스베트라나 이사코바, 드미트리 제메로프
- 옮긴이: 오현석
- 출판사: 에이콘출판
- 읽은 날짜: 2025.03.10 ~ 2025.
- 북 스터디로 진행 (카뱅)

## 1장. 코틀린이란 무엇이며, 왜 필요한가?
- 코틀린은 맨 처음에 '더 나은 자바'로 시작했다.
  - 개발자들이 저지를 수 있는 일반적인 유형의 오류를 방지한다.
  - 현대적 언어 설계 패러다임을 포용한다. (다중 패러다임 언어)
    - 객체지향 언어와 함수형 언어의 아이디어를 조합했다.
  - 자바가 쓰이던 모든 곳에 더 편리하게 쓸 수 있는 언어이다.
- 코틀린은 정적 타입 지정(statically typed) 언어이다.
  - 핵심적인 장점은 프로그램의 모든 식의 타입을 컴파일 시점에 알 수 있다.
  - 컴파일러가 타입을 추론한다.
  - 성능, 신뢰성, 유지 보수성
- 코틀린의 주 목적
  - 현재 자바가 사용되고 있는 모든 용도에 적합하면서도 더 간결하고 생산적이며 안전한 대체 언어를 제공하는 것
  - 더 적은 코드로 더 편하게 목표를 달성한다.

### 코틀린의 철학
- "상호운용성에 초점을 맞춘 실용적이고 간결하며 안전한 언어"
- 실용적이다.
  - 코틀린은 실제 문제를 해결하기 위한 언어이다. 많은 개발자들의 의견을 반영했다.
  - IDE 지원이 좋다. (IntelliJ)
- 간결하다.
  - 코드가 더 간단하고 간결할수록 내용을 파악하기가 더 쉽다. 이때 언어 자체의 간결성도 중요하다.
  - 언어가 간결하다는 것
    - 그 언어로 작성된 코드를 읽을 때 의도를 쉽게 파악할 수 있는 구문 구조를 제공한다.
    - 이해할 때 방해가 될 수 있는 부가적인 준비 코드가 적다.
  - ex. getter, setter, 생성자 파라미터 등을 사용하지 않음. 강력한 타입 추론. 라이브러리 함수.
  - 중요한 것은 읽고 이해하는 데 시간이 덜 걸린다.
- 안전하다.
  - 프로그래밍 언어가 안전하다는 뜻은, 프로그램에서 발생할 수 있는 오류 중에서 일부 유형의 오류를 프로그램 설계가 원천적으로 방지해준다는 뜻이다.
  - JVM에서 실행한다는 사실은 이미 상당한 안정성이 있다.
    - 메모리 안전성을 보장한다.
    - 버퍼 오버플로를 방지한다.
    - 동적으로 할당하나 메모리를 잘못 사용하는 문제를 예방할 수 있다.
    - 읽기 전용 변수를 쉽게 정의할 수 있다. 이는 다중 스레드 애플리케이션에서 안전성을 더 보장한다.
  - 컴파일 시점에서 오류를 더 방지한다.
    - `NullPointerException`
      - 코틀린 타입 시스템은 null이 될 수 없는 값을 추적하며 실행 시점에 NPE가 발생할 수 있는 연산을 사용하는 코드를 금지시킨다.
    - 클래스 타입 캐스팅
      - 코틀린은 타입 검사와 캐스트를 한 연산자로 수행한다.
- 상호운용성이 좋다.
  - 자바의 기존 라이브러리를 사용할 수 있다.
  - 자바 코드에서 코틀린 코드를 호출할 때도 아무런 노력이 필요하지 않다.

## 2장. 코틀린 기초
- '코틀린스러운' 코드란?
  - 코틀린에서 제공하는 syntactic sugar를 적절히 사용해 코드를 작성하는 것.
  - 코틀린 커뮤니티에서 일반적으로 받아들여지는 프로그래밍 스타일과 잘 들어맞고, 언어 설계자들이 권장하는 방식을 따르는 코드.

### 문(statement)와 식(expression)의 구분
- statement
  - 자신을 둘러싼 가장 안쪽 블록의 최상위 요소로 존재
  - 아무런 값을 만들어내지 않는다.
  - ex. 루프(for, while)
- expression
  - 값을 만들어낸다.
  - 다른 식의 하위 요소로 계산에 참여할 수 있다.

### 식 본문 함수
- 코틀린에서는 식 본문 함수(expression body function)이 자주 쓰인다.
- 반환 타입을 명시하지 않아도 컴파일러가 타입을 추론한다.
  - 블록 본문 함수는 타입 명시가 필수다.
- 라이브러리를 작성할 때는 반환 타입을 명시하라.

### 2.2 행동과 데이터 캡슐화: 클래스와 프로퍼티
- 프로퍼티(property) = 필드와 접근자(getter)
- `val`, `var` 모두 비공개 필드를 갖는다.
- 보통 프로퍼티에는 그 값을 저장하기 위한 필드가 있다. (backing field)
- 원한다면 프로퍼티 값을 그때그때 계산할 수도 있다. (커스텀 접근자)

### 2.3 enum과 when
- when도 if와 마찬가지로 값을 만들어내는 식(expression)이다.
- when 식의 대상 값을 변수에 넣을수도 있다.
  - ex `when(val color = someMethod()) {...}`

## 5. 람다를 사용한 프로그래밍

### 람다란
- 다른 함수에 전달할 수 있는 작은 코드 조각
- 일련의 동작을 다른 함수에 넘기거나 변수에 저장하기 위해 사용
- 자바에서는 예전에 익명 내부 클래스를 사용했지만 너무 번거로워 자바 8에서 람다가 등장하였다.
- 람다를 사용하면 더 간결하게 표현할 수 있다. 함수 선언을 할 필요가 없다.

### 코틀린 람다
- 람다가 인자를 하나만 사용하고, 구체적인 이름을 붙이고 싶지 않다면 `it`이라는 암시적 이름을 사용할 수 있다.
- 단순히 함수나 프로퍼티에 위임할 경우 멤버 참조(`::`)를 사용할 수 있다.
- 항상 중괄호(`{}`)로 둘러싸여 있다.
  - 인자 주변 목록에 괄호가 없다.
  - 화살표(`->`)가 인자와 람다 본문을 구분한다.
- 코드의 일부분을 블록으로 둘러싸 실행하고 싶다면 `run {}`을 사용하라.
- 컴파일러가 문맥으로 유추 가능한 인자 타입을 굳이 적지 않아도 된다.
  - 컴파일러가 타입을 유추할 수 없을 때만 명시하자.
- 람다가 어떤 함수의 유일한 인자이거나 마지막 인자인 경우, 빈 괄호(`()`)를 생략할 수 있다.
  - ex) `map() {}`(X) `map {}`(O)
- 둘 이상의 람다를 인자로 받는 함수는 람다를 괄호 밖으로 빼낼 수 없다.
- 람다를 변수에 저장할 때는 파라미터의 타입을 추론할 문맥이 존재하지 않아 타입을 명시해야 한다.
- 본문이 여러 줄인 경우, 본문의 맨 마지막에 있는 식이 람다의 결괏값이 된다. 명시적인 `return`이 필요하지 않다.

### 변수 캡처(capturing variables)
- 함수 안에 익명 내부 클래스를 선언하면, 그 클래스 안에서 함수의 파라미터와 로컬 변수를 참조할 수 있다. 람다 안에서도 동일한 일을 할 수 있다.
- 람다를 함수 안에서 정의하면 함수의 파라미터뿐 아니라 람다 정의보다 앞에 선언된 로컬 변수까지 람다에서 모두 사용할 수 있다.
- 코틀린과 자바 람다의 다른 점 중 하나는 코틀린 람다 안에서는 파이널 변수(불변 변수)가 아닌 변수에 접근할 수 있다는 것이다.
  - 람다 안에서 바깥의 변수를 변경해도 된다.
  - 람다 안에서 접근할 수 있는 외부 변수를 '람다가 캡처한 변수'라고 한다.
- 기본적으로 함수 안에 정의된 로컬 변수의 생명주기는 함수가 반환되면 끝난다.
  - 하지만 어떤 함수가 자신의 로컬 변수를 캡처한 람다를 반환하거나 다른 변수에 저장한다면 로컬 변수의 생명주기와 함수의 생명주기가 달라질 수 있다.
- 코틀린에서는 캡처한 변수가 있는 람다를 저장해서 함수가 끝난 뒤에 실행해도 람다의 본문 코드는 여전히 캡처한 변수를 읽거나 쓸 수 있다.
  - 파이널 변수를 캡처한 경우 - 람다 코드를 변수 값과 함께 저장
  - 파이널이 아닌 변수를 캡처한 경우 - 변수를 특별한 래퍼로 감싸서 나중에 변경하거나 읽을 수 있게 한다음, 래퍼에 대한 참조를 람다 코드와 함께 저장한다.
- 람다를 비동기적으로 실행되는 코드로 활용하는 경우, 로컬 변수 변경은 람다가 실행될 때만 일어난다.

### 멤버 참조(member reference)
- 함수를 값으로 바꿀 수 있다. 이중 콜론(`::`)을 사용한다. 이를 멤버 참조라고 한다.
- 정확히 한 메서드를 호출하거나 한 프로퍼티에 접근하는 함수 값을 만들어준다.
- 최상위 함수나 프로퍼티를 참조하는 경우, `::method` 식으로 참조 가능하다.
- 생성자 참조는 클래스 생성 작업을 연기하거나 저장해둘 수 있다. `::`뒤에 클래스 이름을 넣으면 생성자 참조를 만들 수 있다.
- 확장함수도 멤버 참조와 동일하게 접근할 수 있다.

### 단일 추상 메서드(Simple Abstract Method) 인터페이스
- 추상 메서드가 단 하나뿐인 인터페이스
- 함수형 인터페이스를 파라미터로 받는 모든 자바 메서드에 람다를 전달할 수 있다.
- 이 때, 함수형 인터페이스에 맞는 익명 클래스를 컴파일러가 구현한다.
  - 람다가 자신이 정의된 함수의 변수에 접근하지 않는다면, 함수가 호출될 때마다 람다에 해당하는 익명 객체가 재사용된다. (그때그때 만들어지지 않는다.)
  - 람다가 자신을 둘러싼 환경의 변수를 캡처하면 더 이상 각각의 함수 호출에 같은 인스턴스를 재사용할 수 없다.
    - 이런 경우 컴파일러는 호출마다 새로운 인스턴스를 만들고 그 객체 안에 캡처한 변수를 저장한다.
- `inline` 표시가 되있는 람다는 익명 클래스가 생성되지 않는다.
  - 대부분의 라이브러리 함수에는 `inline`이 붙어있다.
- 대부분의 경우 람다를 함수형 인터페이스 인스턴스로 변환하는 과정은 자동으로 일어난다.
- SAM 생성자는 SAM 인터페이스의 인스턴스로 명시적으로 변환해준다. ex) `Runnalbe { some() }`
  - 컴파일러가 변환을 자동으로 수행하지 못하는 맥락에서 사용할 수 있다.
- 람다는 인스턴스 자신을 가리키는 `this`를 사용할 수 없다. `this`가 필요한 경우, 람다 대신 익명 객체를 직접 구현한다.

### 코틀린에서 SAM 인터페이스 정의: `fun interface`
- 코틀린 함수형 인터페이스는 정확히 하나의 추상 메서드만 포함하지만 다른 비추상 메서드를 여럿 가질 수 있다.

### 수신 객체 지정 람다: `with, apply, also`
- 수신 객체를 명시하지 않고 람다의 본문 안에서 다른 객체의 메서드를 호출할 수 있게 한다.
- `with`
  - `public inline fun <T, R> with(receiver: T, block: T.() -> R): R` 
  - 어떤 객체의 이름을 반복하지 않고도 그 객체에 대해 다양한 연산을 수행하는 기능
  - 첫 번째 인자로 받은 객체를 두 번째 인자로 받은 람다의 수신 객체로 만든다. 그래서 `this` 접근이 가능하다. (생략도 가능하다.)
  - `with`가 반환하는 값은 람다 코드를 실행한 결과이다. 그 결과는 람다식의 본문에 있는 마지막 식의 값이다.
- `apply`
  - `public inline fun <T> T.apply(block: T.() -> Unit): T`
  - `with`와 거의 동일하다. 유일한 차이는, `apply`는 항상 자신에 전달된 객체를 반환한다. `(T) -> T`
  - 인스턴스를 만들면서 즉시 프로퍼티 중 일부를 초기화해야 하는 경우 `apply`가 유용하다.
    - 자바의 빌더와 비슷하다.
- `also`
  - `public inline fun <T> T.also(block: (T) -> Unit): T`
  - 수신 객체에 대해 어떤 동작을 수행한 후 수신 객체를 돌려준다.
  - 주된 차이는 `also`의 람다 안에서는 수신 객체를 인자로 참조한다는 점이다.
    - 따라서 파라미터 이름을 부여하거나 디폴트 이름인 it을 사용해야 한다.
  - 원래 수신 객체를 인자로 받는 동작을 실행할 때 유용하다. 객체에 대해 추가 작업을 실행할 수 있다.
    - "그리고(also) 다음을 객체에게 수행한다."로 읽을 수 있다.

## 6. 컬렉션과 시퀀스

- 람다를 인자로 받는 함수에 내부 로직의 복잡도를 생각하라. 연산이 반복되지는 않는지 고려하라.
- 항상 작성하는 코드로 인해 어떤 일이 벌어질지 명확히 이해해야 한다. 

### 컬렉션 함수
- 원소 제거와 변환: `filter`, `map`
  - `filter`: 컬렉션을 순회하며 주어진 람다가 true를 반환하는 원소들만 모은다.
    - `filterIndexed`: 원소의 인덱스를 포함하여 추출
    - `filterKeys`: map에서 키 기준 추출
    - `filterValues`: map에서 값 기준 추출
  - `map`: 입력 컬렉션의 원소를 변환한다.
    - `mapIndexed`: 원소의 인덱스를 포함한 `map` 연산
    - `mapKeys`: map에서 키를 변환 (Map<K1, V) -> Map<K2, V>)
    - `mapValues`: map에서 값을 변환 (Map<K, V1> -> Map<K, V2>)
- 컬렉션 값 누적: `reduce`, `fold`
  - 컬렉션의 정보를 종합한다.
  - 원소로 이뤄진 컬렉션을 받아서 한 값을 반환한다. 이 값은 accumulator(누적)을 통해 점진적으로 만들어진다.
  - 람다는 각 원소에 대해 호출되며 새로운 누적 값을 반환해야 한다.
  - `reduce`
    - 1번째 원소를 accumulator에 넣어서 시작한다.
    - 빈 리스트에서 호출하면 Exception 발생
  - `fold`
    - `reduce`와 비슷하지만, 초기값을 설정할 수 있다.
  - `runningReduce`, `runningFold`
    - `reduce`와 `fold`이 최종 결과값만 반환한다면, running이 붙은 함수들은 모든 중간 결과를 포함한 리스트를 반환한다.
- 컬렉션에 술어 적용: `all`, `any`, `none`, `count`, `find`
  - `all`: 모든 원소들이 조건을 만족하는지 판별
  - `any`: 원소들 중 하나라도 조건을 만족하는지 판별
    - `!all { predicate } == any { !predicate }`
  - `none`: 모든 원소들이 조건을 만족하지 않음을 판별 (=`!any`)
  - empty list 에선 어떻게 동작할까?
    - `all` -> 항상 true (조건을 만족하지 않는 원소를 댈 수 없기 때문)
    - `any` -> 항상 false (조건을 만족하는 원소가 없기 때문)
    - `none` -> 항상 true (any와 반대)
  - `count`: 조건을 만족하는 원소의 개수 반환
    - `filter { }.size`와 중간 과정이 다르다. `filter`는 중간 컬렉션을 만들지만, `count`는 원소 개수만 추적한다.
  - `find`: 조건을 만족하는 첫 번째 원소 반환
    - 원소가 전혀 없으면 `null` 반환 (`firstOrNull`과 같다.)
- 파티션을 분할해 리스트의 쌍으로 만들기: `partition`
  - `partition`: 조건을 만족하는 그룹과 그렇지 않은 그룹으로 나눈다. 전체 컬렉션을 2번 순회하지 않는다.
- 리스트를 여러 그룹으로 이뤄진 맵으로 바꾸기: `groupBy`
  - `groupBy`: 컬렉션의 원소를 어떤 특성에 따라 여러 그룹으로 나눌 때 사용
    - 컬렉션의 원소를 구분하는 특성이 key, 각 그룹이 value가 된다.
- 컬렉션을 맵으로 변환: `associate`, `associateWith`, `associateBy`
  - `associate`
    - 입력 컬렉션의 원소로부터 키/값 쌍을 만들어낸다.
    - 입력 시 중위 함수 `to`를 사용해 키/값 쌍을 표현한다.
  - `associateWith`
    - 컬렉션의 원래 원소를 키로 사용
    - 작성한 람다는 그 원소에 대응하는 값을 만든다.
  - `associateBy`
    - 컬렉션의 원래 원소를 값으로 사용
    - 작성한 람다는 그 원소에 대응하는 키를 만든다.
    - 키가 겹칠 경우 가장 마지막 결과가 저장된다.
- 가변 컬렉션의 원소 변경: `replaceAll`, `fill`
  - `replaceAll`
    - 지정한 람다로 얻은 결과로 컬렉션의 모든 원소를 변경한다.
    - `MutableList`에서 사용 가능하다.
  - `fill`
    - 가변 리스트의 모든 원소를 똑같은 값으로 바꾸는 경우 사용
- 컬렉션의 특별한 경우 처리: `ifEmpty`
  - 컬렉션에 아무 원소가 없을 때 기본값을 생성하는 람다를 제공한다.
- 컬렉션 나누기: `chunked`, `windowed`
  - `chunked`
    - 컬렉션을 주어진 크기로 서로 겹치지 않는 부분으로 나눌 때 사용.
    - 마지막으로 만들어진 청크의 크기는 정한 사이즈보다 작을 수 있다.
  - `windowed`
    - 슬라이딩 윈도우를 생성한다. 정한 길이만큼 인덱스를 1씩 밀어서 리스트들을 생성한다.
- 컬렉션 합치기: `zip`
  - 별도의 두 리스트를 한꺼번에 종합할 때 사용. 종합하면서 변환도 가능하다.
  - 결과 컬렉션은 두 입력 컬렉션 중 더 짧은 쪽의 길이와 같다.
  - `zip`은 항상 2개씩 합칠 수 있다. 중위 연산자로 여러개 겹치더라도 flat하게 합쳐지지 않는다. ex. ((A, B), C)
- 내포된 컬렉션의 원소 처리: `flatMap`, `flatten`
  - `flatMap`
    - 내포된 컬렉션 없이 컬렉션을 만들 수 있다.
    - 주어진 원소들을 변환한 다음, 하나의 리스트로 만든다.
  - `flatten`
    - 변환하는 것 없이 내포된 컬렉션을 하나의 컬렉션으로 만들고 싶을 때 사용

### 시퀀스(Sequence)
- 컬렉션 함수들은 결과 컬렉션을 즉시(eagerly) 생성한다.
  - 연쇄 호출하면, 매 단계마다 계산 중간 결과를 새로운 컬렉션으로 만든다.
- 시퀀스는 자바 스트림처럼 중간 임시 컬렉션을 만들지 않고 연산한다.
  - 시퀀스는 게으르게(lazy) 연산한다.
  - 연쇄적인 연산을 효율적으로 수행한다.
- `asSequence` 확장함수를 통해 어떤 컬렉션이든 시퀀스로 바꿀 수 있다.
- 큰 컬렉션에 대해 연산을 연쇄할 때는 시퀀스를 사용하라.
  - 크기가 충분히 크지 않다면 즉시 계산의 연산 효율이 더 좋다.
- 모든 연산은 각 원소에 대해 순차적으로 적용된다.
- 컬렉션의 경우 연산의 순서가 성능에 영향을 끼친다.
  - 연쇄 연산에서 더 빨리 원소들을 제거(filter)수록 코드의 성능이 더 좋아진다.

### 시퀀스 연산 실행: 중간 연산과 최종 연산
- 중간 연산
  - 다른 시퀀스를 반환
  - 최초 시퀀스의 원소를 변환하는 방법을 안다.
  - 항상 지연 연산
  - 최종 연산이 호출되지 않으면 아무 연산도 하지 않는다.
- 최종 연산
  - 결과를 반환. 모든 연산을 수행.
  - 시퀀스에서 일련의 계산을 수행해 얻을 수 있는 컬렉션이나 원소, 수 또는 다른 객체

### 시퀀스 만들기
- `asSequence()`
  - 컬렉션을 시퀀스로 변환한다.
- `generateSequence`
  - 이전의 원소를 인자로 받아 다음 원소를 계산한다.

## 7. 널이 될 수 있는 값

### 7.1 Nullablility
- NPE를 피할 수 있게 돕는 코틀린 타입 시스템
- 가능한 null 문제를 실행 시점에서 컴파일 시점으로 옮김.
  - 널 여부를 타입 시스템에 추가하여 컴파일러가 관련 오류를 컴파일 시 미리 감지하여, 런타임 예외 가능성을 줄인다.

### 7.2 Null이 될 수 있는 타입
- 코틀린 타입 시스템은 널이 될 수 있는 타입을 명시적으로 지원한다.
- null을 포함하는 타입을 받을 수 있게 하려면 타입 이름에 물음표(?)를 명시해야 한다.
- 물음표가 없다면 null 참조가 불가능하다.
- 모든 타입은 기본적으로 null이 아니다. 명시적으로 ?가 붙어야 null이다.

### 7.3 타입의 의미 자세히 살펴보기
- 타입 = 가능한 값의 집합과, 그런 값들에 대해 수행할 수 있는 연산의 집합
- 특정 타입(클래스)와 null은 엄연히 다르다. 자바 타입 시스템도 서로 다른 것으로 취급한다.
  - `instanceof` 연산자도, null은 String이 아니라고 답한다.
  - null이 아님을 추가로 검사하기 전에 그 변수가 어떤 연산을 할 수 있을지 알 수 없다.
  - `@NotNull`/`@Nullable`이나 `Optional`로도 null을 완벽히 안전하게 처리할 수 없다.
- 코틀린은 널이 될 수 있는 타입과 아닌 타입을 구분하여 연산 가능성을 명확히 한다.
  - 실행시점에서는 널이 될 수 있는 타입과 그렇지 않은 타입은 같다.
  - 래퍼 타입이 아니다. 그래서 널이 될 수 있는 타입을 처리하는 데 실행 시점 부가 비용이 들지 않는다.

### 7.4 안전한 호출 연산자로 null 검사와 메서드 호출 합치기: `?.`
- 안전한 호출 연산자 `?.`
  - null 검사와 메서드 호출을 한 연산으로 수행
  - 호출하는 값이 null이 아니라면 일반 메서드 호출처럼 동작
  - 호출하려는 값이 null이면 null 반환

### 7.5 엘비스 연산자로 null에 대한 기본값 제공 : `?:`
- 엘비스 연산자 `?:`
  - null 대신 사용할 기본값을 지정하는 연산자
  - 첫번째 값이 null이 아니면 그 값을 반환
  - 첫번째 값이 null이면 두번째 값을 반환
- 코틀린에서는 `return`, `throw`도 식이기 때문에 엘비스 연산자 뒤에 쓸 수 있다.

### 7.6 예외를 발생시키지 않고 안전하게 타입을 캐스트하기: `as?`
- `as?`
  - 어떤 값을 지정한 타입으로 변환한다. 대상 타입으로 값을 변환할 수 없으면 null을 반환한다.

### 7.7 널 아님 단언: `!!`
- `!!`
  - 어떤 값이든 널이 아닌 타입으로 바꾼다.
  - 실제 null에 대해 !!를 적용하면 NPE가 발생한다.
  - NPE가 발생하면 사용하는 곳이 아닌 단언문이 선언된 곳을 가리킨다.
    - 스택 트레이스가 몇 번째 줄인지만 알려주기 때문에, 한 줄에 여러 `!!`를 쓰는 일을 피하라.
  - 컴파일러에게 "나는 이 값이 null이 아님을 알고 있으며, 잘못 되었따면 예외가 발생해도 감수하겠다."라는 말하는 것과 같다.

### 7.8 `let` 함수
- `let`
  - `let`을 안전한 호출 연산자(`.?`)와 함께 사용하여, 널이 될 수 있는 값을 널이 아닌 값만 인자로 받는 함수에 넘길 때 자주 사용한다.
  - 긴 식의 결과를 저장하는 변수를 따로 만들 필요가 없다.
  - null이 아닌 경우에만 코드 블록을 실행하고 싶을 때 사용한다.
  - 어떤 식의 결과를 변수에 담되 그 영역을 한정시키고 싶을 때 독립적으로 사용한다.
- `apply`
  - 빌더 스타일을 사용해 객체 프로퍼티를 설정할 때 사용
- `also`
  - 객체에 어떤 동작을 실행한 후 원래의 객체를 다른 연산에 사용하고 싶을 때
- `with`
  - 하나의 객체에 대해 이름을 반복하지 않으면서 여러 함수 호출을 그룹으로 묶고 싶을 때
- `run`
  - 객체를 설정한 다음에 별도의 결과를 돌려주고 싶을 때

### 7.9 직접 초기화하지 않는 널이 아닌 타입: 지연 초기화 프로퍼티
- 코틀린에서는 클래스 안의 널이 아닌 프로퍼티를 생성자 안에서 초기화하지 않고 특별한 메서드 안에서 초기화할 수는 없다.
  - 일반적으로 생성자에서 모든 프로퍼티를 초기화해야 한다.
  - 프로퍼티 타입이 널이 될 수 없는 타입이라면 반드시 널이 아닌 값으로 그 프로퍼티를 초기화해야 한다.
- 이를 해결하기 위해 지연 초기화(lazy-initialize)를 사용할 수 있다.
- `lateinit` 변경자를 붙이면 나중에 초기화할 수 있다.
  - 지연 초기화 프로퍼티는 항상 `var`이어야 한다.
  - `val`은 `final` 필드로 컴파일 되기 때문이다.
  - 반드시 클래스 멤버일 필요는 없고, 함수 본문 안의 지역 변수나 최상위 프로퍼티도 지연 초기화할 수 있다.

### 7.10 안전한 호출 연산자 없이 타입 확장: 널이 될 수 있는 타입에 대한 확장
- 널이 될 수 있는 타입에 대한 확장 함수를 정의하면 null 값을 다루는 강력한 도구로 활용할 수 있다.
  - 메서드 호출이 null을 수신 객체로 받고 내부에서 null을 처리하게 만들 수 있다.

### 7.11 타입 파라미터의 널 가능성
- 코틀린에서 함수나 클래스의 모든 타입 파라미터는 기본적으로 null이 될 수 있다.
  - 타입 파라미터 T를 클래스나 함수 안에서 타입 이름으로 사용하면 이름 끝에 물음표가 없더라도 T가 널이 될 수 있는 타입이다. (즉, T는 `Any?`로 추론된다.)
  - 널이 아님을 확실히 하려면 널이 될 수 없는 타입 상계(upper bound)를 지정해야 한다.
    - `<T: Any>`

### 7.12 널 가능성과 자바
- 자바와 코틀린은 혼용할 때, 널 가능성 어노테이션(`@Nullable`, `@NotNull`)이 없는 경우, 자바의 타입은 코틀린의 플랫폼 타입이 된다.
- 플랫폼 타입
  - 코틀린이 널 관련 정보를 알 수 없는 타입
  - 널이 될 수 있는 타입으로 처리해도 되고, 널이 될 수 없는 타입으로 처리해도 된다. 모든 연산이 허용된다.
- 자바 API를 다룰 때는 조심하라. null이 될 수 있는지 확인해야 한다.
- 자바 클래스나 인터페이스를 코틀린에서 구현할 경우 널 가능성을 제대로 처리하는 일이 중요하다.

## 8장. 기본 타입, 컬렉션, 배열

### 8.1 원시 타입과 기본 타입
- 코틀린은 원시 타입과 래퍼 타입을 구분하지 않는다.
- 래퍼 타입을 따로 구분하지 않으면 편리하다. + 원시 타입의 값에 대해 메서드를 호출할 수 있다.
- 실행 시점에 숫자 타입은 가능한 한 가장 효율적인 방식으로 표현된다.
- 대부분의 경우 코틀린 `Int`는 자바 `int`로 컴파일 된다.
- 코틀린에서 널이 될 수 있는 원시 타입을 사용하면 그 타입은 자바의 래퍼 타입으로 컴파일 된다.
- 제네릭을 쓰는 경우 래퍼 클래스로 컴파일 된다.
  - JVM이 타입 인자로 원시 타입을 허용하지 않기 때문이다.
- 코틀린은 자바와 다르게 한 숫자 타입을 다른 숫자 타입으로 자동 변환하지 않는다. 명시적으로 변환 메서드를 호출해야 한다.
  - (주의) `Long.toInt()`의 경우, Int 범위를 벗어나는 경우 일부를 잘라낸다.
- 리터럴(123L, 2U, 0.42f 등)을 사용한 경우 명시적으로 변환할 필요는 없다.
- 문자열을 수나 boolean으로 변환하기.
  - String.toInt() (숫자 변환 실패 시 Exception 발생)
  - String.toIntOrNull() (숫자 변환 or Null)
  - String.toBoolean (대소문자 구분하지 않음)
  - String.toBooleanStrict (소문자 "true"인 경우만 true)
- `Any`와 `Any?`
  - 코틀린에서는 `Any 타입`이 모든 널이 될 수 없는 타입의 조상
    - `toString()`, `equals()`, `hashCode()` 메서드 포함
    - `Any` 타입에 원시 타입 값을 할당하면 자동으로 박싱된다.
  - 자바에서는 `Object`가 그 역할지만, `int`같은 원시 타입을 포함하지 않는다.
  - `wait`, `notify` 등을 사용하고 싶다면 `Object`로 캐스팅해야 한다.
- `Unit`
  - 자바 `void`와 같은 기능
  - 코틀린에서 메서드의 반환 타입을 명시하지 않으면 `Unit`이 된다.
  - `Unit`은 타입 인자로 쓸 수 있다.
- `Nothing` 타입: 이 함수는 결코 반환되지 않는다.
  - 함수가 정상적으로 끝나지 않았음을 표현
  - `Nothing`은 아무 값도 포함하지 않기 때문에, 함수의 반환 타입이나 반환 타입으로 쓰일 타입 파라미터로만 쓸 수 있다.

### 8.2 컬렉션과 배열
- 널이 될 수 있는 값의 컬렉션과 널이 될 수 없는 값의 컬렉션
  - ex. `List<Int?>?`
    - 리스트 자체가 null이 될 수도 있고, 원소도 null이 될 수도 있다.
  - `filterNotNull`: null이 아닌 원소만 거르는 컬렉션 메서드
- 읽기 전용과 변경 가능한 컬렉션
  - 코틀린은 컬렉션 안의 데이터에 접근하는 인터페이스와 컬렉션 안의 데이터를 변경하는 인터페이스를 분리했다.
    - kotlin `Collection` 인터페이스에는 원소에 접근하는 메서드는 있지만 원소를 추가하거나 제거하는 메서드는 없다.
    - kotlin `MutableCollection` 인터페이스에 컬렉션의 데이터를 수정하는 메서드가 있다.
  - 가능하면 코드에서 항상 읽기 전용 인터페이스를 사용하는 것을 일반적인 규칙으로 삼아라.
    - 컬렉션 변경이 필요할 때만 변경 가능한 버전을 사용하라.
  - 읽기 전용 컬렉션이라도 꼭 변경 불가능하지는 않다.
    - 읽기 전용 컬렉션이 thread-safe하지 않음을 명심하라.
- 코틀린 컬렉션과 자바 컬렉션은 밀접히 연관됨
  - 모든 코틀린 컬렉션은 그에 상응하는 자바 컬렉션 인터페이스의 인스턴스이다.
    - 코틀린과 자바 사이를 오갈 때 아무 변환이 필요 없다.
    - 자바 `Collection` 타입에 코틀린 `Collection`이나 `MutableCollection` 타입을 쓸 수 있다.
  - 코틀린의 컬렉션 인터페이스는 읽기 전용과 변경 가능으로 나뉜다.
    - `ArrayList`는 `MutableList`를, `HashSet`은 `MutableSet`을 상속한 것처럼 취급한다.
  - 자바 코드와 코틀린 코드를 혼용하며 컬렉션을 쓴다면 주의해야 한다.
    - 코틀린에서 null 불가능하거나 수정 불가능이라 하더라도, 자바에서 nullable하거나 수정할 수 있다.
- 자바에서 선언한 컬렉션은 코틀린에서 플랫폼 타입으로 보임
  - 플랫폼 타입의 컬렉션은 코틀린에서 읽기 전용 컬렉션 혹은 변경 가능한 컬렉션 어느 쪽으로도 취급할 수 있다.
  - 주의할 점
    - 컬렉션 자체가 null이 될 수 있는가?
    - 컬렉션의 원소가 null이 될 수 있는가?
    - 컬렉션이 수정 가능한가?
  - 자바 인터페이스나 클래스가 어떤 맥락에서 사용되는지 정확히 알아야 한다.
- 성능과 상호운용을 위해 객체의 배열이나 원시 타입의 배열을 만들기
  - 코틀린에서 배열의 타입을 표현하기 위한 제네릭을 사용한다. 따라서 그 타입 인자는 항상 객체다. (원시 타입일 수 없다.)
  - 원시 타입의 배열을 표현하기 위한 별도 클래스가 있다. ({원시타입명}Array)
    - ex. IntArray, ByteArray ...
    - 이는 제네릭을 사용한 배열과 다르게 박싱하지 않는다.
  - 코틀린에서는 배열에도 컬렉션의 확장 함수가 있다. (`map`, `filter` ...)
    - 다만 반환타입은 `List`임을 유의하라.

## PART 2. 코틀린을 코틀린답게 사용하기

## 9장. 연산자 오버로딩과 다른 관례

- Convention: 코틀린에서 어떤 언어 기능과 미리 정해진 이름의 함수를 연결해주는 것
  - ex. plus 함수는 + 연산자로 사용할 수 있다.

### 9.1 산술 연산자를 오버로드해서 임의의 클래스에 대한 연산을 더 편리하게 만들기
- 연산자를 오버로드하려면 `operator` 키워드를 붙인다.
- 확장함수로도 연산자 오버로드가 가능하다.
- 코틀린에서 미리 정한 연산자만 오버로드 할 수 있다.
- 이항 연산에 필요한 두 인자가 같은 타입일 필요는 없다.
  - 이럴 경우 교환법칙(`a op b == b op a`)이 적용되지 않을 수 있다.
  - 서로 다른 타입에 각각 연산자 오버로드를 통해 교환법칙을 성립하게 만들 수 있다.
- 이항 연산자 종류
  - `+`: plus
  - `-`: minus
  - `*`: times
  - `/`: div
  - `%`: mod
- 기타
  - 자바의 비트 연산자는 코틀린에 없다. 다만, 대응되는 기능을 가진 중위 함수(infix fun)이 존재한다.
- 이항 연산자를 오버로드하면, 복합 대입 연산자를 지원한다.
  - ex) `plus`를 오버로드하면, `+=`도 자동으로 함께 오버로드된다.
- 복합 대입 연산자를 따로 오버로드 하고 싶다면, `{연산}Assign` 등의 함수를 오버로드하면 된다.
  - `plusAssign`, `minusAssign`, `timesAssign` ...
- 일반적으로 이항 연산자와 복합 대입 연산자를 동시에 정의하는 것을 지양한다.
- 단항 연산자도 오버로드할 수 있다.
- 단항 연산자 종류
  - `+`: unaryPlus
  - `-`: unaryMinus
  - `!`: not
  - `++`: inc
  - `--`: dec

### 9.2 비교 연산자를 오버로딩해서 객체들 사이의 관계를 쉽게 검사
- `equals`: `==`, `!=`
  - `==`는equals 호출과 null 검사로 컴파일 된다.
    - `a == b` -> `a?.equals(b) ?: (b == null)`
  - `===`는 두 피연산자가 서로 같은 객체를 가르키는지(동등성)를 비교한다.
    - `===`는 오버로딩할 수 없다.
  - `Any` 클래스에서 `operator` 키워드가 붙기 때문에, 하위 클래스에서는 `override`만 붙이면 된다.
- 순서 연산자: `compareTo`
  - 한 객체와 다른 객체의 크기를 비교해 정수로 나타냄.
  - 코틀린은 비교 연산자(<, >, <=, >=)를 `compareTo`로 컴파일한다.
  - `p1 < p2` 는 `p1.compareTo(p2) < 0`과 같다.
  - `Comparable`에 `operator`가 붙어있으므로, 상속하는 클래스에서는 `override`만 붙인다.
  - `compareValuesBy`: 두 객체와 여러 비교 함수(람다 혹은 메서드 참조)를 인자로 받는다.
  - 필드를 직접 비교하면 코드는 좀 더 복잡해지지만 비교 속도는 훨씬 더 빨라진다.

### 9.3 컬렉션과 범위에 대해 쓸 수 있는 관례
- `[]`: get과 set
  - 인덱스 접근 연산자 `a[b]`는 get 연산
  - 인덱스 접근 연산자에 값을 대입하면 set 연산
  - `operator fun get(...)`으로 함수를 정의하면, `[]`를 통해 원소를 접근할 수 있다.
  - `operator fun set(...)`으로 함수를 정의하면, `[]`를 통해 원소를 대입할 수 있다.
    - set이 받는 마지막 파라미터 값이 대입 연산자의 오른쪽, 나머지 파라미터 값은 인덱스 연산자에 들어간다.
- `in`
  - 원소가 컬렉션이나 범위에 속하는지 검사(contains 메서드)
  - 컬렉션에 있는 원소를 이터레이션 할 때 사용
- `..`: 범위 만들기
  - `rangeTo` 메서드를 정의해 `..`(시작과 끝 포함) 범위를 만들 수 있다.
  - `rangeUntil` 메서드를 정의해 `..<`(시작 포함, 끝 제외) 범위를 만들 수 있다.
  - rangeTo나 rangeUntil은 연산 우선순위가 낮다.
- iterator 관례
  - for 루프는 `in` 연산자를 사용한다.
  - 이때의 `in`은 `iterator()`를 호출한다음, `hasNext`와 `next` 호출을 반복하는 식으로 변환된다.
  - `iterator` 메서드를 확장 함수로 정의해도 사용할 수 있다.

### 9.4 `component` 함수를 사용해 구조 분해 선언 제공
- 구조 분해 선언(destructuring declaration)
  - 복합적인 값을 분해해서 별도의 여러 지역 변수를 한꺼번에 초기화할 수 있다.
- 구조 분해 선언의 각 변수를 초기화하고자 `componentN`이라는 함수를 호출한다.
  - `N`은 구조 분해 선언에 있는 변수 위치에 따라 붙는 번호
- data class의 주 생성자에 들어있는 프로퍼티에 대해서 컴파일러가 자동으로 `componentN` 함수를 만든다.
- 함수에서 여러 값을 반환할 때 유용
- 배열과 컬렉션도 구조 분해 선언을 할 수 있다.
  - 코틀린 표준 라이브러리는 컬렉션의 맨 앞 다섯 원소에 대한 `componentN` 메서드를 제공한다.
- `Pair`나 `Triple` 클래스를 사용한면 더 단순하게 사용할 수 있다.
  - 다만, 그 안에 담겨있는 원소의 의미를 말해주지 않으므로 표현력을 잃게 된다.
- 변수 선언이 들어갈 수 있는 장소라면 어디든 구조 분해 선언을 사용할 수 있다.
- 컴포넌트가 여럿 있는 객체에 대해 구조 분해 선언을 사용할 때, 변수 중 일부가 필요가 없을 경우
  - 변수 이름을 `_`로 할 수 있다.
  - 구조 분해 선언에서 뒤쪽의 구조 분해 선언은 생략할 수 있다.
- 구조 분해 선언 구현은 위치에 의한 것이다. 즉, 구조 분해 연산의 결과가 오직 인자의 위치에 따라 결정된다.
  - 변수의 이름은 중요하지 않다. 그래서 순서를 유의하지 않으면 의도와 다르게 할당 될 수 있다.
  - 복잡한 엔티티에 대해 구조 분해 선언의 사용을 가능한 한 피하라.

### 9.5 프로퍼티 접근자 로직 재활용: 위임 프로퍼티
- 위임 프로퍼티(delegated property)
  - 접근자 로직을 매번 재구현할 필요 없이 쉽게 구현할 수 있다.
  - 위임은 객체가 직접 작업을 수행하지 않고 다른 도우미 객체가 그 작업을 처리하도록 하는 디자인 패턴을 말한다.
    - 이 때 작업을 처리하는 도우미 객체를 위임 객체라고 부른다.
- `var p: Type by Delegate()`
  - p 프로퍼티는 접근자 로직은 다른 객체에 위임한다.
  - Delegate 클래스의 인스턴스를 위임 객체로 사용한다.
  - by 뒤에 있는 식을 계산해 위임에 쓰일 객체를 얻는다.
  - 컴파일러는 숨겨진 도우미 프로퍼티를 만들고 그 프로퍼티를 위임 객체의 인스턴스로 초기화한다.
  - p 프로퍼티는 바로 그 위임 객체에게 자신의 작업을 위임한다.
  ```kotlin
  class Foo {
      private val delegate = Delegate()

      var p: Type
          set(value: Type) = delegate.setValue(..., value)
          get() = delegate.getValue(...)
  }
  ```
  - Delegate 클래스는
    - `getValue`와 `setValue` 메서드를 제공해야 한다.
      - 변경 가능한 프로퍼티만 `setValue`를 사용한다.
    - `provideDelegate`는 선택적으로 구현할 수 있다. 이 함수는 최초 생성 시 검증 로직을 수행하거나 위임이 인스턴스화 되는 방식을 변경할 수 있다.
    - 이런 함수들은 멤버/확장 함수 모두 가능하다.
- `by lazy()`를 사용한 지연 초기화
  - 지연 초기화(lazy initialization)란, 객체의 일부분을 초기화하지 않고 남겨뒀다가 실제로 그 부분의 값이 필요할 경우 초기화할 때 쓰이는 패턴이다.
    - 초기화 과정에서 자원을 많이 사용하거나 객체를 사용할 때마다 꼭 초기화하지 않아도 되는 프로퍼티에 대해 지연 초기화 패턴을 사용할 수 있다.
  - backing property
    - 클래스에서 같은 개념을 표현하는 프로퍼티를 2개 사용하고, 비공개 프로퍼티 앞에 `_`을 붙인다.
  - `lazy { /* getValue 메서드 */ }`
    - 인자는 값을 초기화할 때 호출할 람다.
    - lazy 함수는 thread-safe
    - 필요하면 동기화에 필요한 락을 lazy 함수에 전달할 수도 있고, 다중 스레드 환경에서 사용하지 않을 프로퍼티를 위해 lazy 함수가 동기화를 생략하게 할 수도 있다.
- 위임 프로퍼티 구현
  - `getValue`와 `setValue`를 구현한다. 이 함수들 앞에는 `operator`가 붙는다.
  - 이 함수들은 파라미터 2개를 받는다.
    - `thisRef: Any?`: 설정하거나 읽을 프로퍼티가 들어있는 인스턴스
    - `porp: KProperty<*>`: 프로퍼티를 표현하는 객체
  - `by` 오른쪽에 있는 식이 꼭 새 인스턴스를 만들 필요는 없다. 함수 호출, 다른 프로퍼티, 다른 식이 `by` 오른쪽에 올 수 있다.
    - 다만, 오른쪽 식을 계산한 결과인 객체는 컴파일러가 호출할 수 있는 올바른 타입의 `getValue`와 `setValue`를 반드시 제공해야 한다.
- 위임 프로퍼티는 커스텀 접근자가 있는 감춰진 프로퍼티로 변환된다.
  - Map/MutableMap 인터페이스는 getValue/setValue 확장 함수가 있어 위임을 지원한다.
  - Exposed 프레임워크에서 DB에 대한 Entity 컬럼을 위임으로 표현한다.
