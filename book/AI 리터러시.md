# [AI 리터러시](http://aladin.kr/p/zRUfB)

## PART 1. 이제는 모두의 리터러시
- AI가 우리 삶에 미치는 영향이 커질수록, AI를 제대로 이해하고 활용하는 능력(= AI 리터러시)의 중요성이 커진다.
- 현대 사회의 리터러시(문해력) = "다양한 개념을 잘 이해하고 활용하며 비판적으로 수용할 수 있는 역량"
- AI 리터러시 = "AI기술을 이해하고, 활용하며, 비판적으로 평가할 수 있는 종합적인 능력"

### 프롬프트 엔지니어링
- AI 시스템에 효과적인 명령어나 질문을 제시하여 원하는 결과를 얻어내는 능력
- AI를 제대로 활용하는 사람이 많지 않다.
- 짦은 프롬프트로는 절대 좋은 결과물을 얻을 수 없다.
- 프롬프트에도 어느 정도 공식이 있으며, 그 공식에 맞춰야 원하는 결과를 얻을 가능성이 높다.
- 필요한 정보를 최대한 상세하게 제시해야 한다.
    - 명확하고 구체적으로
- 필수 프롬프트
    - 지시 사항(Instruction)
        - AI가 혼란스럽지 않도록 모호하거나 중의적 표현은 피한다.
        - AI 한계를 인지하고 수행할 수 있는 작업인지 확인한다.
            - 불가능한 작업을 지시하지 않도록 주의하자.
    - 맥락(Context)
        - AI가 작업을 수행하는 데 필요한 배경 정보를 제공
    - 출력 형식(Output Format)
- 보조 프롬프트
    - 입력 데이터
        - 데이터는 구조회된 형태로 제시하는 것이 좋음.
    - 제약 조건
        - 답변에 특정 제한을 두면 더 구체적인 결과를 얻을 수 있다.
        - 글자 수, 사용 언어, 포함하거나 제외해야 할 내용 등
    - 예시
        - 어떤 답변을 원하는지 구체적으로 알려주는 것

### 프롬프트 엔지니어링 고급 기법
- 제로샷 프롬프팅
    - 별도 예시 없이 새로운 작업을 요청
    - AI가 기존에 학습했던 내용을 바탕으로 처음 접하는 작업이나 질문에 대응
    - 복잡하거나 전문적인 작업에는 정확도가 떨어질 수 있다.
    - AI 학습 범위에 벗어나면 답변이 이상할 수 있다.
- 원샷 프롬프팅
    - 단 하나의 예시를 제공하여 작업을 지시
    - 최소한의 정보로 최대한의 결과를 얻고자 할 때 사용
    - 대표성이 높은 예시를 제공하는 것이 좋음.
- 퓨삿 프롬프팅
    - 여러 개의 예시를 제공하여 작업을 지시
- CoT(Chain of Thought) 프롬프팅
    - AI에게 문제 해결 과정을 단계별로 설명하도록 유도
    - AI가 복잡한 문제를 해결하는 과정을 보여줌.
- 역할 할당 프롬프팅
    - AI에게 특정 역할이나 관점을 부여, 그 역할에 맞는 방식으로 응답을 생성하도록 유도
- 위의 프롬프팅을 조합하여 사용하면 좋다.
- (모델마다 효과적인 프롬프팅이 조금씩 다르다. 모델별로 자신만의 프롬프트 노하우를 익혀가야 한다.)
- 한 번에 한 가지 주제만 물어보는 것이 좋다.
    - (다만, 맥락이 너무 길어지면 AI의 답변 시간이나 퀄리티가 떨어질 수 있다.)
- AI에게 역으로 효과적인 프롬프트가 무엇인지 물어보라.

### AI에 대한 비판적 사고
- AI가 생성한 결과물을 비판적으로 평가하는 역량이 매우 중요하다.
- AI 환각
    - AI가 거짓 정보를 생성하는 것
    - AI가 만들어낸 정보는 생각보다 부정확하다.
- AI는 강력하지만 보조 매체이다. 답변은 참고 정도만 하는 것이 좋다.
- 프롬프팅을 할 때, "이 정보가 공개되어도 괜찮은가?" 생각하고 AI에 질문하라.
- AI가 배우는 정보는 우리 사회의 편견이 그대로 담겨 있을 확률이 높다.

### AI 디바이드
- "AI 격차"가 현실화되고 있다. AI를 잘 쓰는 사람과 못 쓰는 사람의 경쟁력 차이가 커질 것이다.
- AI를 꾸준히 학습할 것
- 신규 AI 서비스가 나오면 한 번은 써볼 것
- 자신의 업무 등 필요한 분야에 다양하게 활용해 보기


## PART 2. 누구나 쉽게 이해하는 인공지능 기술
- 추천 알고리즘을 경험하면서 생각할 점
    - 추천이 잘못된 경우, 왜 잘못되었는지 생각해보기. (내가 어떤 행동을 했는지)
    - 더 나은 추천을 받기 위해 의도적으로 행동하기.
    - 내 개인 정보가 어떻게 활용되고 있는지 생각해보기.
- AI: 인간의 지능을 모방하여 학습, 문제 해결, 패턴 인식 등을 수행할 수 있는 컴퓨터 시스템을 연구하고 개발하는 분야
- 기계 학습(Machine Learning): 컴퓨터가 스스로 경험을 통해 배우고 이를 바탕으로 새로운 데이터를 처리할 수 있게 하는 기술
- 강화 학습: AI가 특정 작업을 수행한 뒤 그 결과에 따라 보상이나 벌점을 받으며 학습하는 방식
- 딥러닝: 여러 층의 인공 신경망을 쌓아 복잡한 패턴을 학습하고 문제를 해결
    - 학습 과정이 블랙 박스처럼 불투명해서 AI가 왜 그런 결정을 내렸는지 정확히 설명하기 어려운 단점이 있음
- 언어 모델: 컴퓨터가 글을 읽고 쓰는 법을 배우는 방법
- LLM의 기반, 트랜스포머
    - 셀프 어텐션 메커니즘: 문장 속 각 단어가 다른 단어들과 얼마나 관련이 있는지 숫자로 나타내는 어텐션 점수를 계산
    - 병렬 처리, 서로 멀리 떨어진 단어들 사이의 관계 이해, 단어의 위치를 고려한 문맥 파악에 강점
- GPT의 학습 과정
    - 사전 학습(Pre-training)
        - 대규모 텍스트를 읽으며 언어의 기본적인 구조와 패턴 학습
        - 다음에 올 문장이나 단어를 예측하는 방식으로 학습
    - 미세 조정(Fine-tuning)
        - GPT를 특정 작업에 맞게 추가 학습(질문-답변 시스템에 적절한 답변을 학습)
    - 강화학습(RLHF)
        - 인간 피드백 강화 학습. 모델의 출력을 인간이 평가하고, 그 평가를 바탕으로 모델을 더욱 개선

## PART 3. AI 리터러시 업그레이드 생성형 AI 서비스 가이드
- 사용법은 쉽다.
- AI 서비스를 활용하는 것은 현대인의 AI 능력 중 가장 중요한 부분
- 시간을 아끼고, 작업의 퀄리티를높이고, 새로운 아이디어를 고안하는데 도움을 준다.
- '적절한' 도구를 사용하는 것이 관건
    - 각 도구의 특징을 이해하고 목적에 맞게 선택

### AI 소개
|이름|특징|
|--|--|
|ChatGPT|가장 높은 점유율, 일반적으로 쓰기 좋음, 데이터 분석에 강점|
|Claude|글쓰기에 강점|
|Gemini|구글 서비스와 연동이 강점, 엄청난 양의 텍스트를 한 번에 처리 가능, 웹 검색을 통한 최신 정보 답변 가능|
|Copliot|MS 서비스(Office)와 연동이 강점|
|클로바X|우리나라에 최적화된 AI, 네이버 서비스와 연계|
|NotebookLM|여러 문서를 한 번에 읽고 인사이트 찾기 가능|
|Perplexity|웹 검색 기반 답변, 참고 문헌 검색(환각 최소화). 출처 명시.|
|Wrks AI|직장인 업무 관련, 맞춤형 AI 비서(RAG) 생성|
|Vizcom|스케치 렌더링|
|Dream Machine|텍스트 to 고품질 영상|
|Capcut|영상 편집|
|Gamma|텍스트 to 프레젠테이션|
|Lilys AI|링크 하나로 빠르고 정확하게 긴 영상 요약|
|Daglo|음성 기반 회의록, 내용 정리|
|Brandmark|로고 생성|
|SCISPACE|논문 탐색, 요약, 비교|
|DeepL|가장 자연스러운 번역|
|Napkin AI|텍스트 to 그래프, 인포그래픽, 흐름도 등 시각화|
|뤼튼|무료로 GPT 최신 모델 활용|
|POE|여러 AI 모델 다양하게 활용하고 싶을 때|
|genspark|최신 뉴스/자료를 모아 블로그 형태의 요약된 문서 생성|
|Cursor AI|프로그래밍|

- AI 발전 동향을 꾸준히 파악하고, 새로운 도구들을 적극적으로 탐색하고 익히기

## PART 4. '나' 맞춤 리터러시
- 감마(Gamma): 프레젠테이션 생성
- 비즈컴(Vizcom): 스케치 기반 렌더링
- 젬(Gem): 자기소개서 생성
- 겟지피티(GetGPT): 모의면접 및 예상 질문
- 다글로(Daglo): 녹음을 통한 회의록 생성
- Napkin AI: 시각화 자료 생성
- 클로드 아티팩트: 데이터 분석 및 시각화
- WrksAI: '나만의 비서 만들기' (자신이 원하는 기능 커스텀)
- LilysAI: 영상, 웹사이트, PDF, 음성 파일, 텍스트 등의 지식 소스를 자동으로 분석하고 핵심 내용을 요약
- NotebookLM: 업로드한 자료를 기반으로 요약. 질의 가능
- dream machine: 영상 생성
- capcut: 영상 편집
- 마이크로소프트 디자이너: 이미지 생성및 편집
- Suno: 작곡
- Skybox: 특정 장소의 360도 파노라마 이미지 생성
- HeyGen: 강의 영상 생성 (가상의 아바타 혹은 자기 자신으로 만든 아바타를 강사로 사용 가능)
- DeepL: 해외 논문, 보고서 번역
- DeepL Write: 글 교정, 첨삭
- SciSpace: AI를 통해 연구 논문 검색, 요약, 질문, 집필, 출판. 논문끼리 비교 가능
- ResearchRabbit: 선행 연구 검토와 논문 탐색, 연구 간 관계 파악
- Perplexity: 웹 자료 검색 및 정리, 출처 명시.
    - 다른 생성형 AI가 만들어낸 내용에 대해 '팩트 체크 해줘'로 검증이 어느정도 가능하다.
- Brandmark: 로고 디자인 생성(사업체명, 슬로건 입력)
- Adobe Firefly: 실사 스타일 이미지 생성
- Immersity: 이미지 업로드하면, 3D 모션 영상으로 변경
- Vrew: 영상 생성&편집
- 모든 AI 서비스를 다 알 필요는 없다. 다만, 나의 상황과 고민에 맞는 AI를 찾아 제대로 활용할 줄 알아야 한다.

## 마무리하며
- AI는 빠르게 우리 삶에 스며들 것이다.
- AI사용을 귀찮아하거나 두려워하지 않기.
- AI를 활용하며 역량을 발전시켜야 한다.
