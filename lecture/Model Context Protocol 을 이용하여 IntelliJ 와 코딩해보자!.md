# [[Sionic MCP 시리즈 1] Model Context Protocol 을 이용하여 IntelliJ 와 코딩해보자!](https://www.inflearn.com/course/scionic-mcp-series1)
- 강사: Sionic AI
---
- MCP 연동 후, JVM에 연결하는 것이 목표

### MCP 연동
- claude - 설정 - 개발자 - 설정 편집
  - claude_desktop_config.json 편집
    - jetbrains 연동
  - 이후 클로드 재시작
- IntelliJ에서 MCP Server 플러그인 다운로드
- 클로드에서 프롬프트로 인텔리제이와 연결해달라고 하면 연동됨.

### 구현
- 컴파일 오류가 나는 부분도 오류 메시지를 AI에게 프롬프트로 주면 알아서 고쳐준다.
- 코드 품질을 떠나서 빠르게 개발이 된다!
- AI 실수를 많이 한다. Stack Trace를 넘겨주면 수정해줄 수 있지만, 결과를 확인하는 편이 낫다.
- 레퍼런스가 없는 기술일수록 AI가 잘못된 코드를 만들 가능성이 높다.

### 나가며
- 생성형 AI는 그 특성상 균일한 답을 하지 못한다.
- 생성형 AI가 생성한 코드는 오류가 잦다.
- 그러나 그럼에도 AI를 활용한 코딩이 훨씬 더 빠르다!
- 실무에서는 작은 단위로 AI에게 코딩을 요청하고, 검수하는 것이 좋다.
