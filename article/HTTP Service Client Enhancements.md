# [HTTP Service Client Enhancements](https://spring.io/blog/2025/09/23/http-service-client-enhancements)
- Spring Boot 4 소개글 3편

- Spring 6에서 도입된 `@HttpExchange` 기반의 선언적 HTTP 클라이언트가 Spring 7에서 `HTTP Service Registry`로 완성되며, 대규모 멀티-클라이언트 구성을 그룹 단위로 표준화,자동화
- Spring Boot 4는 이 등록된 Registry를 통해 RestClient/WebClient 빌더를 자동으로 초기화
- Spring Cloud 2025.1에서 로드 밸런싱과 서킷 브레이커 기능을 AutoConfiguration
- 흔히 쓰는 OpenFeign 프로젝트가 유지 보수 위주로 개발되고 있고, 스프링과 자동으로 연계하기 위해서 Http Service Client를 추상화
- 앞으로는 OpenFeign 의존성 없이, Spring 의존성만으로도 Http 통신을 가능하게 하려는 큰 그림
