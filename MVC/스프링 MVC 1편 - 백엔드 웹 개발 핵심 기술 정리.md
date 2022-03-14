> 김영한님의 [스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1) 강의 내용을 정리한 것입니다.

# 2. 서블릿
- 서블릿은 톰캣 같은 웹 애플리케이션 서버(WAS)를 직접 설치하고, 그 위에 서블릿 코드를 클래스 파일로 빌드해서 올린 다음, 톰캣 서버를 실행하면 된다.
- 스프링 부트는 톰캣 서버를 내장하고 있으므로, 톰캣 서버 설치 없이 편리하게 서블릿 코드 실행 기능
#### 스프링 부트 서블릿 환경 구성
- `@ServletComponentScan` : 서블릿을 직집 등록
### 서블릿 동작 방식
- 스프링 부트를 통해 생성된 내장 톰캣 서버의 서블릿 컨테이너 안에서 등록, 설정한 서블릿이 생성됨
- 웹 브라우저에서 요청을 보내면, 웹 애플리케이션 서버에서 HTTP 요청 메시지를 기반으로 request와 response를 생성
- 이를 서블릿 컨테이너로 넘겨주면, 서블릿에서 request 객체와 response 객체로 받고 내부 로직을 실행
- 실행된 결과를 Response 객체에 적절히 담고, 웹 애플리케이션 서버에 보내주면, Reponse 객체 정보로 HTTP 응답을 생성
- 이를 다시 웹 브라우저에 전송

### HttpServletRequest 역할
- 서블릿은 개발자가 HTTP 요청 메시지를 편리하게 사용할 수 있도록 개발자 대신에 HTTP 요청 메시지를 파싱한다. 그리고 그 결과를 `HttpServletRequest` 객체에 담아서 제공한다.
- `HttpServletRequest`를 사용하면 다음과 같은 HTTP 요청 메시지를 편리하게 조회 가능
  - START LINE
    - HTTP 메소드
    - URL
    - 쿼리 스트링
    - 스키마, 프로토콜
  - 헤더
    - 헤더 조회
  - 바디
    - form 파라미터 형식 조회
    - message body 데이터 직접 조회
- 부가 기능
  - 임시 저장소 기능 : 해당 HTTP 요청이 시작부터 끝날 때 까지 유지되는 임시 저장소 ㅡㅇ
    - 저장 : `request.setAttribute(name, value)`
    - 조회 : `request.getAttribute(name)`
  - 세션 관리 기능
    - `request.getSession()`
- HttpServletRequest, HttpServletResponse 등을 잘 이해하려면 HTTP 스펙이 제공하는 요청, 응답 메시지 자체를 이해해야 함.

### HTTP 요청 데이터 - GET 쿼리 파라미터 조회 메서드
```java
// 단일 파라미터 조회
String username = request.getParameter("username")
// 파라미터 이름들 모두 조회
Enumeration<String> parameterNames = request.getParameterNames();
// 파라미터를 Map으로 조회
Map<String, String[]> parameterMap = request.getParameterMap();
// 복수 파라미터 조회
String[] usernames = request.getParameterValues("username");
```

### HTTP 요청 데이터 - POST HTML Form
- `application/x-www-form-urlencoded`형식은 앞서 GET에서 살펴본 쿼리 파라미터 형식과 같음
- 따라서 쿼리 파라미터 조회 메서드를 그대로 사용하면 된다. (`request.getParameter()`)

### HTTP 요청 데이터 - API 메시지 바디 - 단순 텍스트
- HTTP 메시지 바디의 데이터를 InputStream을 사용해서 직접 읽을 수 있음.
```java
@WebServlet(name = "requestBodyStringServlet", urlPatterns = "/request-body-string")
public class RequestBodyStringServlet extends HttpServlet {
  @Override
  protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    ServletInputStream inputStream = request.getInputStream();
    String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
  }
}
```
- `InputStream`은 byte 코드를 반환한다. byte 코드를 우리가 읽을 수 있는 문자(String)로 보려면 문자표(String)를 지정해주어야 한다.

### HTTP 요청 데이터 - API 메시지 바디 - JSON
- content-type: application/json
- JSON 형식으로 파싱할 수 있게 객체를 생성한다. (ex HelloData)
```java
@WebServlet(name = "requestBodyJsonServlet", urlPatterns = "/request-body-json")
public class RequestBodyJsonServlet extends HttpServlet {
  private ObjectMapper objectMapper = new ObjectMapper();
  
  @Override
  protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    ServletInputStream inputStream = request.getInputStream();
    String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
    
    HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
    }
}
```
- JSON 결과를 파싱해서 사용할 수 있는 자바 객체를 변환하려면 Jackson, Gson 같은 JSON 변환 라이브러리를 추가해서 사용해야 한다. 
- 스프링 부트로 SpringMVC를 선택하면 기본으로 Jackson 라이브러리(`ObjectMapper`)를 함께 제공한다.

### HttpServletResponse 역할
- HTTP 응답코드 지정
- 헤더 생성
- 바디 생성
- 편의 기능 제공
  - Content-Type (`setContentType()`,`setCharacterEncoding()`)
  - 쿠키 (`addCookie()`)
  - Redirect(`sendRedirect()`)

### HTTP 응답 데이터 - 단순 텍스트, HTML
- `PrintWriter writer = reseponse.getWriter(); writer.println()`
- 단순 텍스트 응답
- HTML 응답 (content-type: text/html)

### HTTP 응답 데이터 - API JSON
```
@WebServlet(name = "responseJsonServlet", urlPatterns = "response-json")
public class ResponseJsonServlet extends HttpServlet {

  private ObjectMapper objectMapper = new ObjectMapper();
  
  @Override
  protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  
    //Content-Type: application/json
    response.setHeader("content-type", "application/json");
    response.setCharacterEncoding("utf-8);
    
    HelloData data = new HelloData();
    data.setUsername("kim");
    data.setAge(20);
    // {"username":"kim,"age":20}
    String result = objectMapper.writeValueAsString(data);
    
    reponse.getWriter().write(result);
  }
  
}
```
- HTTP 응답으로 JSON을 반환할 때는 content-type을 `application/json`으로 지정해야 한다.
- Jackson 라이브러리가 제공하는 `objectMapper.writeValueAsString()`을 사용하면 객체를 JSON 문자로 변경할 수 있다.


# 3. 서블릿, JSP, MVC 패턴
### 템플릿 엔진이 나온 이유
- 자바 코드로 HTML을 만들어 내는 것이 매우 복잡하고 비효율적이다.
- 차라리 HTML 문서에 동적으로 변경해야 하는 부분만 자바 코드를 넣는 것이 더 편리
- 템플릿 엔진을 사용하면 HTML 문서에서 필요한 곳만 코드를 적용해서 동적으로 변경할 수 있음
- JSP, Thymeleaf 등

### 서블릿과 JSP의 한계
- JSP를 보면, JAVA 코드, 데이터를 조회하는 리포지토리 등등 다양한 코드가 모두 노출되어 있다.
- JSP가 너무 많은 역할을 한다.
- 비즈니스 로직은 서블릿처럼 다른 곳에서 처리하고, JSP는 목적에 맞게 HTML로 화면(View)를 그리는 일에 집중하도록 설계해야 한다. 이러한 고민 끝에 MVC 패턴이 등장했다.

### MVC 패턴 - 개요
#### 너무나 많은 역할
- 하나의 서블릿이나 JSP만으로 비즈니스 로직과 뷰 렌더링까지 모두 처리하게 되면, 너무 많은 역할을 하게 되고, 결과적으로 유지보수가 어려워진다.
#### 변경의 라이프 사이클
- 진짜 문제는 비즈니스 로직과 뷰 렌더링 사이에 변경의 라이프 사이클이 다르다는 점이다.
- UI를 일부 수정하는 일과 비즈니스 로직을 수정하는 일은 각각 다르게 발생할 가능성이 매우 높고 대부분 서로에게 영향을 주지 않는다.
- 변경의 라이프 사이클이 다른 부분을 하나의 코드로 관리하는 것은 유지보수하기 좋지 않다.
#### 기능 특화
- JSP 같은 뷰 템플릿은 화면 렌더링 하는데 최적화되어 있기 때문에 이 부분의 업무만 담당하는 것이 가장 효과적이다.
#### Model View Controller
- MVC 패턴은 컨트롤러(Controller)와 뷰(View)라는 영역으로 서로 역할을 나눈 것을 말한다.
- 웹 애플리케이션은 보통 이 MVC 패턴을 사용한다.
#### 컨트롤러
- HTTP 요청을 받아서 파라미터를 검증하고, 비즈니스 로직을 실행한다. 
- 그리고 뷰에 전달할 결과 데이터를 조회해서 모델에 담는다.
#### 모델
- 뷰에 출력할 데이터를 담아둔다.
- 뷰가 필요한 데이터를 모두 모델에 담아서 전달해주는 덕분에 뷰는 비즈니스 로직이나 데이터 접근을 몰라도 되고, 화면을 렌더링 하는 일에 집중할 수 있다.
#### 뷰
- 모델에 담겨있는 데이터를 사용해서 화면을 그리는 일에 집중한다.
- 여기서는 HTML을 생성하는 부분을 말한다.
#### 서비스
- 컨트롤러에 비즈니스 로직을 둘 수도 있지만, 이렇게 되면 컨트롤러가 너무 많은 역할을 담당한다.
- 그래서 일반적으로 비즈니스 로직은 서비스(Service)라는 계층을 별도로 만들어서 처리한다.
- 그리고 컨트롤러는 비즈니스 로직이 있는 서비스(Service)를 호출하는 담당한다.
- 참고로 비즈니스 로직을 변경하면 비즈니스 로직을 호출하는 컨트롤러의 코드도 변경될 수 있다.

### MVC 패턴 - 적용





