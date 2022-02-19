> 참고자료  
> https://articles09.tistory.com/29  
> https://velog.io/@sungmo738/Resource-Autowired-Inject-%EC%B0%A8%EC%9D%B4

# @Autowired, @Resource, @Inject의 차이
## 의존 객체 자동 주입(Automatic Dependency Injection)
- 스프링 설정파일에서 혹은 태그로 의존 객체 대상을 명시하지 않아도 스프링 컨테이너가 자ㅗㅇ적으로 의존 대상 객체를 찾아 해당 객체에 필요한 의존성을 주입하는 것

## @Resource
- Java에서 지원하는 어노테이션, 특정 프레임 워크에 종속적이지 않음.
- 찾는 순서 `이름 -> 타입 -> @Qualifire -> 실패`
- name 속성의 이름을 기준으로 찾음. 없으면 타입, 없으면 @Qualifire 어노테이션의 유무를 찾아 의존성을 주입
- xml 설정에 `<context:annotation-config/>` 설정을 반드시 추가해야 함
- @Resource 어노테이션의 값으로 빈 객체의 이름을 지정
- 생성자에 적용할 수 없고, 필드(멤버변수)나 메서드(setter)에만 사용 가능

## @Autowired
- Spring에서 지원하는 어노테이션
- 찾는 순서 `타입 -> 이름 -> @Qualifire -> 실패`
- 주입하려고 하는 객체의 타입이 일치하는지를 찾고, 객체를 자동으로 주입
- 만약 타입이 존재하지 않는다면 @Autowired에 위치한 속성명이 일치하는 빈을 컨테이너에서 찾음.
- 이름이 없을 경우 @Qualifire 어노테이션의 유무를 찾아 의존성을 주입
- xml 설정에 `<context:annotation-config/>` 설정을 반드시 추가해야 함
- 필드(멤버 변수), 메서드(setter), 생성자, 일반 메소드에 적용 가능

## @Inject
- Java에서 지원하는 어노테이션, 특정 프레임 워크에 종속적이지 않음.
- 찾는 순서 `타입 -> @Qualifire -> 이름 -> 실패`
- @Autowired와 동일하게 작동하지만 찾는 순서가 다름
- maven이나 gradle에 javax 라이브러리 의존성을 추가해야 함
- 필드(멤버 변수), 메서드(setter), 생성자, 일반 메소드에 적용 가능

## @Qualifire
