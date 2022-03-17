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
- `dispatcher.forward()` : 다른 서블릿이나 JSP로 이동할 수 있는 기능이다. 서버 내부에서 다시 호출이 발생한다.
- `/WEB-INF` : 이 경로 안에 JSP가 있으면 외부에서 직접 JSP를 호출할 수 없다. 항상 컨트롤러를 통해서 JSP를 호출하는 것이 가능해진다.
- redirect vs forward
  - 리다이렉트는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청한다.
  - 따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경된다.
  - 반면에 포워드는 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다.
- HttpServletRequest를 Model로 사용한다.
  - request가 제공하는 `setAttribute()`를 사용하면 request 객체에 데이터를 보관해서 뷰에 전달할 수 있다.
  - 뷰는 `request.getAttribute()`를 사용해서 데이터를 꺼내면 된다.

### MVC 패턴 - 한계
#### MVC 컨트롤러의 단점
- 포워드 중복
  - View로 이동하는 코드가 항상 중복 호출되어야 한다. 물론 이 부분을 메서드로 공통화해도 되지만, 해당 메서드도 항상 직접 호출해야 한다.
```java
RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
dispatcher.forward(request, response);
```
- ViewPath 중복
  - prefix : `/WEB-INF/views`
  - suffix : `.jsp`
  - 그리고 만약 jsp가 아닌 thymeleaf 같은 다른 뷰로 변경한다면 전체 코드를 다 변경해야 한다.
- 사용하지 않는 코드
  - HttpServletRequest, HttpServletResponse를 사용할 때도 있고, 사용하지 않을 때도 있다. 그리고 이를 사용하는 코드는 테스트 케이스를 작성하기도 어렵다.
- 공통 처리가 어렵다.
  - 기능이 복잡해질수록 컨트롤러에서 공통으로 처리해야 하는 부분이 점점 더 많이 증가할 것이다.
  - 단순히 공통 기능을 메서드로 뽑으면 될 것 같지만, 결과적으로 해당 메서드를 항상 호출해야 하고, 실수로 호출하지 않으면 문제가 될 것이다.
  - 그리고 호출하는 것 자체도 중복이다.
- 정리하면 공통 처리가 어렵다는 문제가 있다.
  - 이 문제를 해결하려면 컨트롤러 호출 전에 먼저 공통 기능을 처리해야 한다.
  - -> 이를 해결하기 위해 프론트 컨트롤러(Front Controller) 패턴을 도입 (입구를 하나로!)
  - 스프링 MVC의 핵심이 바로 이 프론트 컨트롤러에 있다.

# 4. MVC 프레임워크 만들기
### 프론트 컨트롤러 패턴 소개
#### FrontController 패턴 특징
- 프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 받음
- 프론트 컨트롤러가 요청에 맞는 컨트롤러를 찾아서 호출
- 입구를 하나로
- 공통 처리 가능
- 프론트 컨트롤러를 제외한 나머지 컨트롤러는 서블릿을 사용하지 않아도 됨
- 스프링 웹 MVC의 DispatcherServlet이 Front Controller 패턴으로 구현되어 있음 (스프링 웹 MVC의 핵심)

### 프론트 컨트롤러 도입 - v1
- 프론트 컨트롤러 단계적 도입
- 기존 코드를 최대한 유지하면서, 프론트 컨트롤러를 도입하는 단계
- 점진적인 리팩터링
- 서블릿과 비슷한 모양의 컨트롤러 인터페이스를 도입
- 구현체와 관계없이 로직의 일관성을 가져갈 수 있음

### View 분리 - v2
```
String viewPath = "WEB-INF/views/new-form.jsp";
RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
dispatcher.forward(request, reponse);
```
- 모든 컨트롤러에서 뷰로 이동하는 부분에 중복이 있고, 깔끔하지 않으므로, 이를 분리하기 위해 별도로 뷰를 처리하는 객체를 만들어서 처리.

### Model 추가 - v3
#### 서블릿 종속성 제거
- 컨트롤러 입장에서 `HttpServletRequest`, `HttpServletResponse`가 항상 꼭 필요하지는 않음
- 요청 파라미터 정보는 자바의 Map으로 대신 넘기도록 하면 지금 구조에서는 컨트롤러가 서블릿 기술을 몰라도 동작할 수 있다.
- 그리고 request 객체를 Model로 사용하는 대신에 별도의 Model 객체를 만들어서 반환하면 된다.
- 구현할 컨트롤러가 서블릿 기술을 전혀 사용하지 않도록 변경
- 구현 코드가 매우 단순해지고, 테스트 코드 작성이 쉬워짐
#### 뷰 이름 중복 제거
- 컨트롤러는 뷰의 논리 이름을 반환하고, 실제 물리 위치의 이름은 프론트 컨트롤러에서 처리하도록 단순화
- 향후 뷰의 폴더 위치가 함께 이동해도 프론트 컨트롤러만 고치면 됨
- 예 : /WEB-INF/views/members.jsp -> members
#### ModelView
- 서블릿의 종속성을 제거하기 위해 Model을 대체하기 위한 객체를 직접 만들기.
- Model + View 이름까지 전달하는 역할

### 단순하고 실용적인 컨트롤러 - v4
- v3 컨트롤러는 서블릿 종속성을 제거하고 뷰 경로의 중복을 제거하는 등, 잘 설계된 컨트롤러다.
- 그러나 컨트롤러 인터페이스를 구현하는 과정에서 항상 ModelView 객체를 생성하고 반환해야 하는 부분이 반복된다.
- 좋은 프레임워크는 아키텍처도 중요하지만, 그와 더불어 실제 개발하는 개발자가 단순하고 편리하게 사용할 수 있어야 한다. 실용성이 있어야 한다.
- 컨트롤러가 `ModelView` 대신에 `ViewName`만 반환하도록 설계

### 유연한 컨트롤러 - v5
#### 어댑터 패턴
- 어댑터 패턴을 사용해서 프론트 컨트롤러가 다양한 방식의 컨트롤러를 처리할 수 있도록 할 수 있다.
- 서블릿이 다양한 컨트롤러 인터페이스를 사용할 수 있도록 변경
#### 핸들러 어댑터
- 다양한 종류의 컨트롤러를 호출할 수 있도록 프론트 컨트롤러와 컨트롤러 중간에 위치하여 어댑터 역할을 하는 것
#### 핸들러
- 컨트롤러를 추상화한 것.
- 어댑터가 있기 때문에 꼭 컨트롤러의 개념 뿐만아니라 어떤 것이든 해당하는 종류의 어댑터만 있으면 다 처리할 수 있기 때문

# 5. 스프링 MVC - 구조 이해
### DispatcherServlet 구조 살펴보기
- 스프링 MVC도 프론트 컨트롤러 패턴으로 구현되어 있다.
- 스프링 MVC의 프론트 컨트롤러가 `DispatcherServlet`
- 스프링 부트는 `DistpatcherServlet`을 서블릿으로 자동으로 등록하면서 모든 경로에 대해서 매핑한다.
  - 더 자세한 경로가 우선순위가 높다. 그래서 기존에 등록한 서블릿도 함께 동작한다.

### 요청 흐름
- 서블릿이 호출되면 `HttpServlet`이 제공하는 `service()`가 호출된다.
- 스프링 MVC는 `DispatcherServlet`의 부모인 `FrameworkServlet`에서 `service()`를 오버라이드해둔 상태에서,
- `FramewordServlet.service()`를 시작으로 여러 메서드가 호출되면서 `DispatcherServlet.doDispatcher()`가 호출된다.

### DispatcherServlet.doDispatch() 코드 분석
```
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {

    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    ModelAndView mv = null;

    // 1. 핸들러 조회
    mappedHandler = getHandler(processedRequest);
    if (mappedHandler == null) {
        noHandlerFound(processedRequest, response);
        return;
    }

    // 2. 핸들러 어댑터 조회 - 핸들러를 처리할 수 있는 어댑터
    HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

    // 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환
    mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

    processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
}


private void processDispatchResult(HttpServletRequest request, HttpServletResponse response, 
                                   HandlerExecutionChain mappedHandler, ModelAndView mv, 
                                   Exception exception) throws Exception {
    // 뷰 렌더링 호출
    render(mv, request, response);
} 


protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {

    View view;
    String viewName = mv.getViewName();

    // 6. 뷰 리졸버를 통해서 뷰 찾기, 7. View 반환
    view = resolveViewName(viewName, mv.getModelInternal(), locale, request);

    // 8. 뷰 렌더링
    view.render(mv.getModelInternal(), request, response);
}
```

### SpringMVC 동작 순서
1. 핸들러 조회 : 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.
2. 핸들러 어댑터 조회 : 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.
3. 핸들러 어댑터 실행 : 핸들러 어댑터를 실행한다.
4. 핸들러 실행 : 핸들러 어댑터가 실제 핸들러를 실행한다.
5. ModelAndView 반환 : 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환해서 반환한다.
6. viewResolver 호출 : 뷰 리졸버를 찾고 실행한다.
7. View 반환 : 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.
8. 뷰 렌더링 : 뷰를 통해서 뷰를 렌더링한다.

### 스프링 MVC - 시작하기
- `@Controller`
  - 스프링이 자동으로 스프링 빈으로 등록한다. 내부에 `@Component`어노테이션이 있어 컴포넌트 스캔의 대상이 된다.
  - 스프링 MVC에서 어노테이션 기반 컨트롤러로 인식한다. 
- `@ReqeustMapping`
  - 요청 정보를 매핑한다. 해당 URL이 오출되면 이 메서드가 호출된다.
  - 어노테이션을 기반으로 동작하기 때문에 메서드의 이름은 임의로 지으면 된다.
  - 우선순위가 가장 높은 핸들러 매핑과 핸들러 어댑터는 `RequestmappingHandlerMapping`, `RequestMappingHandlerAdapter`
  - 실무에서 99.9% 이 방식의 컨트롤러 사용
- `ModelAndView`
  - 모델과 뷰의 정보를 담아서 반환하면 된다.
- `RequestMappingHandlerMapping`은 스프링 빈 중에서 `@RequestMapping` 또는 `@Controller`가 클래스 레벨에 붙어 있는 경웨 매핑 정보로 인식한다.

### 스프링 MVC - 실용적인 방식
- 소스코드에서 V3 방식이 실무에서 주로 사용하는 방식
- 메서드마다 Model 파라미터를 편하게 받을 수 있다.
- ViewName(뷰의 논리 이름) 직접 반환
- `@RequestParam` 사용
  - 스프링은 HTTP 요청 파라미터를 `@RequestParam`으로 받을 수 있다.
  - GET 쿼리 파라미터, POST Form 방식을 모두 지원
- `@RequestMapping` -> `@GetMapping`, `@PostMapping`
  - `@RequestMapping`은 URL만 매칭하는 것이 아니라, HTTP Method도 함께 구분할 수 있다.

# 6. 스프링 MVC - 기본 기능
### 매핑 정보
- `@Controller`는 반환값이 String이면 뷰 이름으로 인식된다. 그래서 뷰를 찾고 뷰가 렌더링 된다.
- `@RestControlelr`는 반환 값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다. 따라서 실행 결과로 ok 메세지를 받을 수 있다. `@ResponseBody`와 관련있다.
- `@RequestMapping("/~")`은 /~ URL이 호출되면 이 메서드가 실행되도록 매핑한다. 대부분의 속성을 배열[]로 제공하므로 다중 설정이 가능하다.
  - HTTP 메서드를 지정하지 않으면 HTTP 메서드와 무관하게 호출된다. 즉 GET, HEAD, POST, PUT, PATCh, DELETE 모두 허용한다.
  - 허용한 메서드 이외의 메서드를 지정하면 HTTP 405 상태코드(Method Not Allowed)를 반환한다.
  - 경로 변수의 경우 최근 HTTP API는 리소스 경로에 식별자를 넣는 스타일을 선호한다.
  - `@PathVariable`의 이름과 파라미터 이름이 같으면 생략할 수 있다.
  - ContentType 헤더를 기반으로 미디어 타입 조건 매핑을 할 수 있으며, 만약 지정한 미디어 타입과 맞지 않은 요청이 들어오면 HTTP 415 상태코드(Unsupported Media Type)을 반환한다.
  - Accept 헤더 기반으로 미디어 타입을 매핑할 수 있으며, 만약 지정한 타입과 맞지 않은 요청이 들어오면 HTTP 406 상태코드(Not Acceptable)을 반환한다.
  - `@RequestParam`을 경우에 따라 생략할 수도 있는데, 너무나 많이 생략하는 감이 있어 명확하게 요청 파라미터에서 데이터를 읽는다는 것을 명시해주는 것이 좋다.


#### 로그 레벨
- LEVEL : TRACE > DEBUG > INFO > WARN > ERROR

### 로그 사용의 장점
- 쓰레드 정보, 클래스 이름 같은 부가 정보를 함께 볼 수 있고, 출력 모양을 조정할 수 있다.
- 로그 레벨에 따라 개발 서버에서는 모든 로그를 출력하고, 운영서버에서는 출력하지 않는 등 로그를 상황에 맞게 조절할 수 있다.
- 시스템 아웃 콘솔에만 출력하는 것이 아니라, 파일이나 네트워크 등, 로그를 별도의 위치에 남길 수 있다. 특히 파일로 남길 때는 일별, 특정 용량에 따라 로그를 분할하는 것도 가능하다.
- 성능도 일반 System.out 보다 좋다. (내부 버퍼링, 멀티쓰레드 등) 그래서 실무에서는 꼭 로그를 사용해야 한다.

### @ModelAttribute
- 스프링MVC는 `@ModelAttribute`가 있으면 다음을 실행한다.
- `@ModelAttribute`뒤의 객체를 생성한다.
- 요청 파라미터 이름으로 해당 객체의 프로퍼티를 찾는다. 그리고 해당 프로퍼티의 setter를 호출해서 파라미터의 값을 입력(바인딩)한다.
  - 예) 파라미터 이름이 username이면 setUsername()메서드를 찾아서 호출하면서 값을 입력한다.

### HTTP 요청 메시지 - 단순 텍스트
- 요청 파라미터와 다르게, HTTP 메시지 바디를 통해 데이터가 넘어오는 경우는 `@RequestParam`, `@ModelAttribute`를 사용할 수 없다.
  - 물론 HTML Form 형식으로 전달되는 경우는 요청 파라미터로 인정된다.
- HTTP 메시지 바디의 데이터를 `InputStream`을 사용해서 직접 읽을 수 있다.
- `HttpEntity`를 상속받은 다음 객체들도 같은 기능을 제공한다.
  - `RequestEntity` : HttpMethod, url 정보가 추가됨, 요청에서 사용
  - `ResponseEntity` : HTTP 상태 코드 설정 가능, 응답에서 사용
- `@RequestBody`
  - HTTP 메시지 바디 정보를 편리하게 조회라 수 있다.
  - 참고로 헤더 정보가 필요하다면 `HttpEntity`를 사용하거나 `@RequestHeader`를 사용
  - 요청 파라미터를 조회하는 `@RequestParam`이나 `@ModelAttribute`와는 관계 없음
- 요청 파라미터 vs HTTP 메시지 바디
  - 요청 파라미터를 조회하는 기능 : `@RequestParam`, `@ModelAttribute`
  - HTTP 메시지 바디를 직접 조회하는 기능 : `@RequestBody`
- `@ResponseBody`
  - 응답 결과를 HTTP 메시지 바디에 직접 담아서 전달할 수 있으며, view를 사용하지 않음

### HTTP 요청 메시지 - JSON
- HttpServletReqeust를 사용해서 직접 HTTP 메시지 바디에서 데이터를 읽어와서, 문자로 변환한다.
- 문자로 된 JSON 데이터를 Jackson 라이브러리인 `objectMapper`를 사용해서 자바 객체로 변환한다.
- `@RequestBody`는 생략 불가능 (생략하면 `@ModelAttribute` 적용)
- 스프링은 `@ModelAttribute`, `@RequestParam` 해당 생략시 다음과 같은 규칙을 적용한다.
  - String, int, Integer 같은 단순 타입 = `@RequestParam`
  - 나머지 = `@ModelAttribute`(argument resolver로 지정해둔 타입 외)
- `@RequestBody` 요청 : JSON 요청 -> HTTP 메시지 컨버터 -> 객체
- `@ResponseBody` 응답 : 객체 -> HTTP 메시지 컨버터 -> JSON 응답

### HTTP 응답 - 정적 리소스, 뷰 템플릿
- 스프링(서버)에서 응답 데이터를 만드는 방법은 크게 3가지
  - 정적 리소스
  - 뷰 템플릿 사용 (동적 HTML)
  - HTTP 메시지 사용 (HTTP 메시지 바디에 JSON 같은 형식으로 데이터 송신)

#### 정적 리소스
- 스프링 부트는 클래스패스의 다음 디렉토리에 있는 정적 리소스를 제공
  - `/static`, `/public`, `/resources`, `/META-INF/resources`
- 리소스 보관장소 : `src/main/resources`
- 정적 리소스 경로 : `src/main/resources/static`
  - 정적 리소스는 해당 파일을 변경 없이 그대로 서비스한다.

#### 뷰 템플릿
- 뷰 템플릿을 거쳐서 HTML이 생성되고, 뷰가 응답을 만들어서 전달
- 스프링부트 기본 뷰 템플릿 경로 `src/main/resources/templates`
- String을 반환하는 경우 - View or HTTP 메시지
- `@ResponseBody`, `HttpEntity`를 사용하면 HTTP 메시지 바디에 직접 응답 데이터 출력 가능

### HTTP 응답 - HTTP API, 메시지 바디에 직접 입력
- `@RestController` 어노테이션 사용시, 해당 컨트롤러에 모두 `@ResponseBody`가 적용된다.

### HTTP 메시지 컨버터 
#### HTTP 요청 데이터 읽기
- HTTP 요청이 오고 컨트롤러에서 `@RequestBody`, `HttpEntity` 파라미터 사용
- 메시지 컨버터가 메시지를 읽을수 있는지 확인하기 위해 `canRead()`를 호출
  - 대상 클래스 타입을 지원하는지
  - HTTP 요청의 Content-Type 미디어 타입을 지원하는지
- `canRead()` 조건을 만족하면 `read()`를 호출해서 객체를 생성하고 반환한다.

#### HTTP 응답 데이터 생성
- 컨트롤러에서 `@ResponseBody`, `HttpEntity`로 값이 반환된다.
- 메시지 컨버터가 메시지를 쓸 수 있는지를 확인하기 위해 `canWrite()`를 호출한다.
  - 대상 클래스 타입을 지원하는가
  - HTTP 요청의 Accept 미디어 타입을 지원하는가
- `canWrite()` 조건을 만족하면 `write()`를 호출해서 HTTP 응답 메시지 바디에 데이털르 생성한다.

### 요청 매핑 핸들러 어댑터 구조
178~



