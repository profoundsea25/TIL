# [생성형 AI와 금융의 만남, 대출 음성 상담 챗봇 서비스](https://tech.kakaopay.com/post/loan-voice-ai-chatbot/)

- 개발 목표: 자연스러운 음성 인터페이스를 가진, 개인 맞춤형 대출 상담 서비스
  - "대출" 특성 상 상담 내용의 데이터 정확성이 매우 중요함.

### 데이터 플로우
- 사용자의 음성 입력
  - FE -> BE (WebSocket 스트리밍)
- 음성을 텍스트로 변환
  - BE -> Amazon Transcribe (스트리밍 API 활용)
- 텍스트를 사용자에게 전달
  - BE -> FE (사용자의 음성이 제대로 인식 되었는지 보여주기 위함)
- LLM 프롬프트/응답
  - BE <-> Amazon Bedrock (다양한 LLM을 쉽게 사용하도록 API 형태로 제공)
- LLM 응답을 텍스트로 사용자에게 전달
  - BE -> FE (텍스트로도 응답 내용 확인)
- 텍스트를 음성으로 변환
  - BE -> Amazon Polly (AWS의 텍스트 음성 변환 서비스)
- 음성을 사용자에게 전달
  - BE -> FE

### 백엔드 구현 세부사항
- 데이터 통신은 WebSocket 스트리밍 활용
- 코틀린 코루틴 채널 활용
  - 비동기 처리 흐름을 단순하게 유지하기 위함.

### AI Agent 파이프라인
1. Synthetic Data Generation: 합성데이터 생성
  - 고객에게 최적으리 방식으로 대출 상품을 소개하기 위해서는 대출 상품과 고객의 행동 및 메타 데이터(성별, 연령 등) 간의 정교한 매칭이 필수
  - 대출 데이터는 민감한 개인 정보를 포함하기 때문에, 원본 데이터를 AI에 학습시킬 수 없음.
  - 개인 정보 보호 규제를 준수하면서 AI를 학습시키려면, 원본 데이터와 유사한 통계적 속성을 지닌 합성 데이터를 생성하는 것이 중요.
  - 하둡으로 가명 처리한 데이터를 뽑아, 대출과 상관관계를 분석한다.
  - 이상치 제외, 데이터의 규칙, 범위 등을 설정
2. Embedding to Knowledge Base
  - 대출 상품과 고객 관련 정보가 업데이트될 때마다 외부 지식 베이스를 최신 상태로 유지하는 RAG 방식 채택
    - 정보를 임베딩 벡터로 변환하는 과정이 필요
  - 추천 우선순위와 상품 개수를 내부 프롬프트 엔지니어링으로 결정

---

### Comment
- 전화가 편한 세대가 더 많기 때문에, 음성 인터페이스로 서비스하는 것은 좋은 생각이다.
- 개인 정보를 대체하는 유의미한 정보를 찾아 개인화된 서비스를 만들 수 있다.
- AI는 결과 예측이 쉽지 않아, 여러 번의 테스트를 거쳐 범위를 제한하는 것이 중요해 보인다.
