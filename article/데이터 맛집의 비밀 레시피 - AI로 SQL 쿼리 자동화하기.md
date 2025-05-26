# [MCP 개념 및 LINE Messaging API를 활용한 MCP 서버 구축 사례 소개](https://techblog.lycorp.co.jp/ko/introduction-to-mcp-and-building-mcp-server-using-line-messaging-api)

- MCP: LLM이 외부 리소스에 접근하기 위한 표준(프로토콜)
    - MCP 호스트: MCP를 사용하는 주체. 즉 LLM
    - MCP 클라이언트: 호스트 내부 모듈로 MCP 서버에 요청
    - MCP 서버: 호스트 외부 모듈. MCP 클라이언트로부터 받은 요청에 응답
- 구성요소
    - 툴: MCP 서버가 제공하는 기능
    - 리소스: MCP 서버가 제공할 수 있는 다양한 형태의 데이터나 콘텐츠
- 작동
    1. MCP 호스트가 실행될 때, tools/list 엔드포인트를 호출해 MCP 서버로부터 가능한 툴 목록을 받아온다.
    2. 사용자가 프롬프트를 한다.
    3. 프롬프트를 분석해 툴이 필요한 경우 해당 MCP 서버에 LLM이 요청을 보낸다.
    4. MCP 서버로부터 받은 응답을 반영해 사용자 응답을 생성한다.
