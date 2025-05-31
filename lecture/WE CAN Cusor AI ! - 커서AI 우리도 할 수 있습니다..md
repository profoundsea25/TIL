# [WE CAN Cusor AI ! - 커서AI 우리도 할 수 있습니다.](https://www.inflearn.com/course/we-can-cusor-ai)
- 강사: 이프로
---

### Cursor AI
- Control + J 눌러서 터미널을 열 수 있음.
- 웹 개발을 위해 Node.JS 설치 필요.
- Control + L 로 AI 패널을 열 수 있다.
- Control + Shift + I 로 컴포즈 열 수 있음.
- AI 모델은 Claude 추천
- 프롬프트 팁
  - 이전에 했던 동작을 알려준다. (~은 잘 되었어.)
  - 지시 사항 작성
- 프롬프트 시, Control + Enter를 사용하면 코드베이스를 모두 컨텍스트로 활용한다.

### Vercel
- Next.js를 개발하는 회사.
- github repository를 import하여 바로 배포가 가능하다.
- 무료 이용 가능

### Composer
- Chat - 질문과 답변 위주
- Composer - AI가 직접 코딩해주는 기능
- 내가 원하는 결과가 아니라면, 정확하게 현상을 설명하고 수정 사항을 요구해야 한다.
  - 사진을 포함해서 지시해도 좋은 방법이다.

### Tips
- 요구사항 구체화도 AI에게 맡겨볼 수 있다.
- 어떻게 더 적은 프롬프트로 내가 원하는 결과물을 얻을 수 있을지 고민하기.
- ChatGPT Generate 기능에서 프롬프트마다 기본으로 설정할 지시(instruction)을 상세하게 생성할 수 있다.
- shardcn/ui
  - next.js 개발자가 많이 사용하는 UI 라이브러리
- cursor에 @docs 추가
- UI는 shardcn 사용한다고 프롬프트에 명시
- AI가 내 의도를 잘 이해할 수 있게 프롬프트를 다듬는 것이 프롬프트 엔지니어링이다.
- AI에게 역할을 부여하기. (당신은 Kotlin 전문가 입니다. 등)
- versel cli로 배포해놓으면, commit마다 자동으로 배포를 할 수 있다.
- openAI Docs에서 프롬프트 엔지니어링 관련 내용 확인해보기
- LLM 애플리케이션 내에 코딩되는 프롬프트는 영어로 작성하는 것을 추천
  - 토큰도 적게 들고, 더 좋은 품질의 답변을 받을 가능성이 높다.
  - 마지막에 한국어로 대답하라고만 쓰면 된다.
- 요즘은 퓨샷(예제 여러개)가 아니더라도, 원샷(예제 1개) 프롬프트 만으로도 효율이 나온다.
- 클로드 3.7과 클로드 코드
  - 클로드 코드를 설치하면 터미널에서 `claude` 명령어가 활성화된다.
  - 다른 에디터를 설치할 필요 없이, 코드 생성을 훨씬 더 빠르게 진행할 수 있다.
  - 프로토타입을 만들기 좋다.
- 프롬프트에 "깊이 생각하여 ~해주세요" 써보기.
- Claude Desktop으로 MCP 사용 추천
- MCP = AI가 외부 리소스에 접근하기 위한 표준 프로토콜

### MCP Server 사이트
- [smithery.ai](http://smithery.ai/)
- [mcp.so](http://mcp.so/)
