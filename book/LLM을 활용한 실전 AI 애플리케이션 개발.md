# [LLM을 활용한 실전 AI 애플리케이션 개발](http://aladin.kr/p/9R7ZW)
- 지은이: 허정준
- 출판사: 책만
- 읽은 날짜: 2025.02. ~ 2025.06.09

## 1부 LLM의 기초 뼈대 세우기
### 1장. LLM 지도
- LLM = 딥러닝 기반 언어 모델
  - LLM은 다음에 올 단어가 무엇일지 예측하면서 문장을 하나씩 만들어 가는 방식으로 텍스트를 생성한다. 이러한 모델을 언어 모델이라고 한다.
- 딥러닝이 머신러닝과 가장 큰 차이를 보이는 것 = "데이터 특징을 누가 뽑는가?"
  - 머신러닝 : 데이터의 특징을 연구자/개발자가 찾고 모델에 입력
  - 딥러닝 : 모델이 스스로 데이터의 특징을 찾고 분류하는 모든 과정을 학습
  - 즉, 데이터를 통해 학습하는 과정에서 그 데이터를 가장 잘 이해할 수 있는 방식을 함께 배운다.
- 임베딩
  - 컴퓨터는 숫자만 처리한다. 따라서 딥러닝 모델은 데이터의 의미와 특징을 숫자의 집합으로 표현한다. 이를 임베딩(embedding)이라 부른다.
  - 임베딩은 거리를 계산할 수 있다. 그래서 검색 및 추천, 분류, 이상치 탐지 등을 할 수 있다.
- 언어 모델링
  - 모델이 입력받은 텍스트의 다음 단어를 예측해 텍스트를 생성하는 방식
- 전이 학습(transfer learning)
  - 하나의 문제를 해결하는 과정에서 얻은 지식과 정보를 다른 문제를 풀 때 사용하는 방식
  - 사전 학습 : 대량의 데이터로 모델을 학습
  - 미세 조정(fine-tuning): 특정한 문제를 해결하기 위한 데이터로 추가 학습
- 정렬(alignment)
  - LLM이 생성하는 답변을 사용자의 요청 의도에 맞추는 것
- LLM 의 미래
  - 멀티 모달 : 다양한 형태의 입력을 받을 수 있는 LLM (이미지, 오디오, 텍스트 ...)
  - 에이전트 : LLM을 활용해 주어진 상황을 인식하고 필요한 행동을 계획하고 의사결정을 내려 필요한 행동을 직접 수행하는 형태

### 2장. LLM의 중추, 트랜스포머 아키텍처 살펴보기
- 트랜스포머 아키텍처
  - 토큰
    - 거의 모든 자연어 처리 연산의 기본 단위
    - 보통 단어보다 짧은 텍스트 단위
  - 토큰화
    - 텍스트를 적절한 단위로 나누고 숫자 아이디를 부여하는 것
  - RNN(순환신경망)
    - 순차적 처리
    - 깊이가 깊어지면 문제가 발생함
  - 셀프 어텐션
    - 순차 처리 방식을 버리고, 입력된 문장 내의 각 단어가 서로 어떤 관련이 있는지 계산하여 각 단어의 표현(representation)을 조정
  - 사람이 글을 이해하는 것처럼 딥러닝 모델이 작동하도록 하려면 단어 사이의 관계를 계산해 관련이 있는지 찾고, 관련이 있는 단어의 맥락을 포함시켜 단어를 재해석해야 한다.
    - 단어 사이의 관련성을 파악하기 위해 토큰 임베딩에 가중치를 도입한다. 이를 통해 내부적으로 토큰과 토큰 사이의 관계를 계산해서 적절히 주변 맥락을 반영하는 방법을 학습한다.
  - 인코더와 디코더로 구성
    - 인코더 집중 모델 : 구글 BERT
      - 자연어 이해에 강점
    - 디코더 집중 모델 : OpenAI GPT
      - 자연어 생성에 강점
    - 인코더-디코더 활용 모델 : 메타 BART, 구글 T5
      - 자연어 이해, 생성을 모두 활용하지만 매우 모델이 복잡하다.
- 주요 사전 학습 매커니즘
  - 인과적 언어 모델링
    - 문장의 시작부터 끝까지 순차적으로 단어를 예측
    - 이전에 등장한 단어들을 바탕으로 다음에 등장할 단어 예측
    - GPT 같은 생성 트랜스포머 모델에서 핵심적인 학습 방법
  - 마스크 언어 모델링
    - 입력 단어의 일부를 마스크 처리하고 그 단어를 맞추는 작업으로 모델을 학습

## 3장. 트랜스포머 모델을 다루기 위한 허길페이스 트랜스포머 라이브러리
- 트랜스포머 아키텍처 등장 이후, 각 회사마다 모델 활용법이 각기 달랐음.
- 허깅페이스 팀에서 개발한 트랜스포머 라이브러리는 공통된 인터페이스로 트랜스포머 모델을 활용할 수 있도록 지원함.
  - 현재 딥러닝 분야의 핵심 라이브러리.

### 허깅페이스 트랜스포머란
- 다양한 트랜스포머 모델을 통일된 인터페이스로 사용할 수 있도록 지원하는 오픈소스 라이브러리
  - 모델을 불러오는 방법, 모델의 함수, 학습 방법 등을 공통화
- 라이브러리
  - `transformers`: 트랜스포머 모델과 토크나이저를 활용할 때 사용
  - `datasets`: 트랜프포머 모델을 쉽게 학습하고 추론에 활용할 수 있도록 돕는다.

### 허깅페이스 허브
- 다양한 사전 학습 모델과 데이터셋을 탐색하고 쉽게 불러와 사용할 수 있도록 제공하는 온라인 플랫폼
- 수많은 사용자들이 자신의 학습한 모델과 데이터셋을 공개

### 허깅페이스 라이브러리 사용법
- 모델을 학습시키거나 추론하기 위해서는 모델, 토크나이저, 데이터셋이 필요하다. 이를 코드로 불러와 사용할 수 있다.
- `AutoModel.from_pretrained()`
  - `AutoModel`은 모델의 바디를 불러오는 클래스
  - `from_pretrained` 메서드에 모델 id를 전달해 적절한 클래스를 가져온다.

## 04. 말 잘 듣는 모델 만들기
- 언어 모델: 다음에 올 단어의 확률을 예측하는 모델

### 지도 미세 조정(supervised fine-tuning)
- LLM이 사용자 요청의 형식을 적절히 해석하고, 응답의 형태를 적절히 작성하며, 요청과 응답이 잘 연결되도록 추가 학습하는 것
- '지도'란 학습 데이터에 정답이 있다는 뜻이다.
- 정렬: LLM이 사용자의 요청에 맞춰 응답하도록 학습하는 것
- 지시 데이터셋(instruction dataset): 지도 미세 조정에 사용하는 데이터셋
  - OpenAI는 레이블러(labeler)를 고용해 지시 데이터셋을 구축했다.
  - 지시 데이터셋의 품질이 높을수록 모델의 답변 품질도 높아진다. 

### 리워드 모델(reward model)
- 지도 미세 조정을 마친 LLM에 지시사항을 입력해 여러 응답을 생성, 레이블러가 응답을 비교해 더 좋다고 판단하는 순서를 정해 선호 데이터셋을 구축한다.

### 강화 학습
  - 에이전트가 환경에서 행동을 한다.
  - 행동에 따라 환경의 상태가 바뀌고, 그 행동에 대한 보상이 생긴다.
  - 에이전트는 변화된 상태를 인식하고 보상을 받는다.
  - 에이전트는 가능하면 더 많은 보상을 받을 수 있도록 행동을 수정하면서 학습한다.
  - 이때 에이전트가 연속적으로 수행하는 행동의 모음을 에피소드라고 한다.

### RLHF(Reinforcement Learning from Human Feedback, 사람의 피드백을 활용한 강화 학습)
- PPO(Proximal Preference Optimization, 근접 정책 최적화): 보상 해킹 피하기
  - 보상 해킹: LLM이 보상을 높게 받는 데만 집중하는 것
  - 지도 미세 조정 모델을 기준으로 학습하는 모델이 너무 멀지 않게 가까운 범위에서 리워드 모델의 점수를 찾도록 한다.
- RLHF는 어렵다.
  - 리워드 모델을 학습시켜야 하는데, 리워드 모델의 성능이 좋지 않으면 일관성이 떨어지며 LLM의 학습이 제대로 되지 않는다.
  - 참고 모델, 학습 모델, 리워드 모델 총 3개가 필요하기 때문에 GPU 같은 리소스가 더 많이 필요하다.

### 강화 학습을 사용하지 않고 사람의 선호를 학습하는 방법
- 기각 샘플링(rejection sampling)
  - 단순히 가장 점수가 높은 데이터를 사용
  - 지도 미세 조정을 마친 LLM을 통해 여러 응답을 생성, 그중에서 리워드 모델이 가장 높은 점수를 준 응답을 모아 다시 지도 미세 조정을 수행한다.
- 직접 선호 최적화(Direct Preference Optimization, DPO)
  - 선호 데이터셋을 직접 학습
  - 선호 데이터셋으로 리워드 모델을 만들고 언어 모델의 출력을 평가하면서 강화 학습을 진행한다.
  - 별도의 리워드 모델이 필요하지 않다.
  - RLHF보다 선호되는 학습 방법

## 2부. LLM 길들이기
## 05. GPU 효율적인 학습
- 기본적으로 GPU에 딥러닝 모델 자체가 올라간다.
  - 딥러닝 모델은 수많은 행렬 곱셈을 위한 파라미터의 집합이다.
- 모델 파라미터의 데이터 타입이 더 많은 비트를 사용할수록 모델의 용량이 커지기 때문에 더 적은 비트로 모델을 표현하는 양자화 기술이 개발됐다.
  - 양자화 기술에서는 더 적은 비트를 사용하면서도 원본 데이터의 정보를 최대한 소실 없이 유지하는 것이 핵심이다.

## 06. sLLM 학습하기
- sLLM: LLM보다 비용 효율적이면서 특정 작업 또는 도메인에 특화된 LLM
- LLM을 활용해 새로운 LLM의 생성 결과를 평가하는 방식이 연구되고 있다.
- OpenAI에서는 대규모 프롬프트 요청을 지원하기 위한 코드를 제공한다.
  - openai-cookbook 리포지토리
  - 티어에 따라 초당 요청 제한량이 다르다.

## 07. 모델 가볍게 만들기
- LLM 서비스의 가장 많은 비용은 GPU 사용에서 발생한다.
- 비용 효율적인 방법
  - 모델의 성능을 약간 희생하더라도 비용을 크게 낮추는 방법
  - 모델의 성능을 그대로 유지하면서 연산 과정의 비효율을 줄이는 방법
- 복습) 언어 모델은 입력한 텍스트 다음에 올 토큰의 확률을 계산하고 그중에서 가장 확률이 높은 토큰을 입력 텍스트에 추가하면서 한 토큰씩 생성한다. (셀프 어텐션 연산)
  - 텍스트 생성 -> 생성한 텍스트 입력 -> 텍스트 생성 -> 생성한 텍스트 입력 -> ... 순환하다가, 생성 종료를 나타내는 토큰을 읽거나, 토큰 길이를 초과할 경우 생성을 종료한다.
    - ex. '고양이가 생선을' -> '고양이가 생선을 먹고' -> '고양이가 생선을 먹고 물을' -> '고양이가 생선을 먹고 물을 마신다' -> ...
- KV 캐시: 셀프 어텐션 연산에서 동일한 입력 토큰에 대해 중복 계산의 비효율을 줄이기 위해 먼저 계산했던 키와 값 결과를 메모리에 저장해 활용
  - ex. 위의 예시에서 '고양이가 생선을'을 캐시
- 지식 증류 = 더 크고 성능이 좋은 선생 모델의 생성 결과를 활용해 더 작고 성능이 낮은 학생 모델을 만드는 방법이다.

## 3부. LLM을 활용한 실전 애플리케이션 개발
## 09. LLM 애플리케이션 개발하기
- RAG(Retrieval Augmented Generation, 검색 증강 생성)
  - 정보를 검색하고 프롬프트를 보강(증강)해서 생성(추론)하는 기술
  - LLM이 답변할 때 필요한 정보를 프롬프트에 함께 전달하는 것
- LLM 추론은 비싸기 때문에, 중복된 추론을 최대한 줄여야 한다. 이를 위해 캐시를 도입하기도 한다.
- 사용자의 요청과 LLM 응답을 반드시 기록해야 한다.
  - 기록하지 않으면 사용자 문의에 대응하기 어렵고, 서비스가 잘 동작하는지 확인할 수 없기 때문이다.
  - 입력이 동일하더라도 추론한 생성 결과가 다를 수 있기 때문에 꼭 기록해야 한다.

### 검색 증강 생성(RAG)
- 답변에 필요한 충분한 정보와 맥락을 제공하고 답변하도록 하는 방법
- 벡터 데이터베이스 사용자 질문과 관련된 정보를 찾아 프롬프트에 통합한다.

### LLM 캐시
- LLM 추론을 수행할 때 사용자의 요청과 생성 결과를 기록하고, 이후에 동일하거나 비슷한 요청이 들어오면 새롭게 텍스트를 생성하지 않고 이전의 생성 결과를 가져와 바로 응답하여 LLM 생성 요청을 줄인다.
- 프롬프트 통합과 LLM 생성 사이에 위치


## 10. 임베딩 모델로 데이터 의미 압축하기
- 맥락 정보를 저장하고 검색하기 위해서는 텍스트를 임베딩 벡터로 변환이 필요하다. 이때 임베딩 모델을 사용한다.
- 의미 검색: 임베딩 벡터의 유사도를 기반으로 검색하는 것
- 의미 검색은 검색할 때 벡터 유사도를 활용하기 때문에 의미상 유사한 문서를 찾을 수 있지만, 문자를 비교하는 키워드 검색 방식보다는 관련도가 떨어지는 결과를 가져올 수 있다.
  - 이런 단점을 보완하기 위해 키워드 검색과 의미 검색을 조합해 사용하는 하이브리드 검색을 사용할 수 있다.
- 텍스트 임베딩/문장 임베딩: 여러 문장의 텍스트를 임베딩 벡터로 변환하는 방식
- 임베딩(embedding): 데이터의 의미를 압축한 숫자 배열(벡터)
  - 컴퓨터가 숫자 형식의 데이터만 연산할 수 있기 때문에, 모든 미디어 데이터는 숫자 형식으로 바꿔야 한다.

## 11. 자신의 데이터에 맞춘 임베딩 모델 만들기: RAG 개선하기
- 교차 인코더는 느리기 때문에 대규모 데이터의 유사도를 계산하는데 사용하지 않는다.
  - 바이 인코더를 통해 선별된 소수의 데이터를 대상으로 더 정확한 유사도 계산을 위해 사용한다.
- 바이 인코더와 교차 인코더를 결합해 사용할 수 있다.
  - 교차 인코더는 비교하려는 두 문장을 입력으로 받아 비교하기 때문에 정밀하지만, 확장성은 떨어진다.
  - 바이 인코더는 독립적인 문장 임베딩 사이의 유사도를 가벼운 벡터 연산을 통해 계산하기 때문에 빠른 검색이 가능하지만, 교차 인코더만큼 정교하지 않다.
- 결합 사용 방법
  1. 바이 인코더를 추가 학습해 검색 성능을 높인다.
  2. 교차 인코더를 추가해 검색 성능을 높일 수 있다.

## 12. 벡터 데이터베이스로 확장하기: RAG 구현하기
### 벡터 데이터베이스
- 벡터 임베딩을 키로 사용하는 데이터베이스
- 벡터 사이의 거리 계산에 특화된 데이터베이스
- 벡터 임베딩 = 데이터의 의미를 담은 숫자 배열(벡터)
  - 모든 데이터는 적절한 임베딩 모델만 있다면 임베딩으로 변환할 수 있다.
- 임베딩 벡터의 거리 계산을 통해 유사하거나 관련이 깊은 데이터를 검색할 수 있다.
  - 비슷한 데이터는 가깝게 있고, 다른 데이터는 멀리 위치하게 되는 임베딩 벡터의 특징을 이용
- 딥러닝과 머신 러닝의 차이 : 직접 사람이 데이터의 특징을 정의하는가?
  - 머신러닝은 사람이 직접 입력 데이터의 특징을 정의해야 한다.
  - 딥러닝은 모델 학습 과정에서 특징을 추출하는 방법도 알아서 학습한다.
- 간단한 벡터 저장과 검색 기능을 구현한다면 벡터 라이브러리만으로도 충분할 수 있도 있다.
  - 보통 벡터 데이터베이스가 벡터 처리에 더 뛰어나다.

### 벡터 데이터베이스 작동 원리
- 벡터 데이터베이스는 벡터 사이의 거리를 계산해 유사한 벡터를 찾는다.
- KNN(K-Nearest Neighbor), 가장 기본적인 방법
  - 저장된 모든 임베딩 벡터를 조사해 가장 유사한 K개 벡터를 반환
  - 직관적이고 모든 데이터를 조사하여 정확하다.
  - 동시에, 연산량이 데이터 수에 비례해 증가하기 때문에 속도가 느려져 확장성이 떨어진다.
- 벡터 검색을 위해서는 인덱스를 만들어야 한다.
  - 관계형 DB의 인덱스와 유사하다.
  - 인덱스의 성능 지표 : 인덱스의 메모리 사용량, 색인 시간, 검색 시간, 재현율
    - 재현율 =  실제로 가까운 K개의 정답 데이터 중 몇 개가 검색 결과로 반환됐는지 그 비율을 나타낸 값.
    - KNN의 재현율은 100% (모든 벡터와 거리를 측정해 결과를 반환하기 때문)

### ANN 검색이란
- ANN(Approximate Nearest Neighbor, 근사 최근접 이웃)
- 대용량 데이터셋에서 주어진 쿼리 항목과 가장 유사한 항목을 효율적으로 찾는데 사용
- 약간의 정확도를 희생하는 대신 훨씬 더 빠른 검색 속도를 제공
- ANN은 임베딩 벡터를 빠르게 탐색할 수 있는 구조로 저장해서 검색 시 탐색할 범위를 좁히는데 집중
- 종류
  - IVF(Inverted File Index)
    - 데이터셋 벡터들을 클러스터로 그룹화한다. 그리고 이것을 활용하여 검색 공간을 제한한다.
    - 인덱싱 시점에 데이터를 가져와 중심점(유사한 벡터들의 클러스터)을 형성한다.
    - 쿼리 시점에는 가장 가까운 중심점을 찾고, 해당 중심점 내에서 가장 가까운 데이터 포인트를 찾는다.
    - 전체 데이터셋 대신 몇 개의 클러스터만 검색하는 것이 전략이다.
    - 검색되지 않은 클러스터의 내용은 놓칠 수 있다.
  - HNSW(Hierarchical Navigable Small World)
    - 그래프 기반 인덱싱 구조
    - 트리 구조 (상위 노드에서 하위 노드로 갈수록 노드 수가 많아진다.)
    - 탐색 속도가 빠르면서 검색 성능이 뛰어나 가장 많이 활용된다.
- HNSW 심화: 탐색 가능한 작은 세계(NSW)
  - 탐색 가능한 작은 세계란, 완전히 랜덤한 그래프와 완전히 규칙적인 그래프 사이에 '적당히 랜덤하게' 연결된 그래프 상태를 말한다.
    - 완전히 규칙적이면 모든 노드가 서로 연결되어 정확하게 탐색 가능하나 검색 시간이 오래 걸린다.
    - 완전 랜덤이면 검색 시간의 편차가 커진다. (어떤 노드는 빠르게 검색하지만, 어떤 노드는 느리게 검색한다.)
  - 완전히 규칙적인 연결에서 일부 연결만 랜덤으로 바꾼다.
  - 정렬된 리스트에서 탐색 속도를 높일 수 있듯(like 이진탐색), 계층 구조를 도입한다.
    - 도입의 이유는, 진입점에서 가장 근접한 벡터를 찾지 못하는 경우가 있기 때문이다.
      - 진입점과 찾고자 하는 벡터 사이에서 벡터와 가장 가까운 지점에 탐색을 멈추는 문제가 있는데, 이를 지역 최솟값(local minimum)이라고 한다.
    - 상위 노드일수록 적지만 크게 이동할 수 있고, 최하위 노드로 가면 모든 노드를 탐색할 수 있다.

## 13. LLM 운영하기
### 13.1 MLOps
- DevOps의 개념을 머신러닝과 데이터 과학 분야로 확장한 방법론
  - DevOps: 소프트웨어 개발과 운영의 협업과 자동화
  - MLOps: DevOps + 데이터와 머신러닝 모델
- MLOps의 목표
  - 데이터 수집, 전처리, 모델 학습, 평가, 배포, 모니터링 등 머신러닝 프로젝트의 전 과정을 자동화하고 효율화하는 것
  - 모델의 재현성(reproducibility)을 보장하는 것이 중요
    - 재현성: 이전에 수행된 ML 워크플로를 그대로 반복했을 때 동일한 모델을 얻을 수 있는지 여부
### 13.2 LLMOps는 무엇이 다를까?
- LLM이 기존 머신러닝 모델에 비해 훨씬 크고 다양한 일을 할 수 있다.
- OpenAI나 구글처럼 일부 기업이 API 기반으로 상업용 모델을 제공한다.
  - 상업용 모델을 사용하면, 쉽게 성능이 높은 모델을 활용할 수 있다.
  - 모델이 바뀌면 잘 작동하던 프롬프트가 작동하지 않는 등 문제가 발생할 수 있다.
  - 사용하는 만큼 비용이 발생한다. 비용 관리가 쉽지 않다.
  - 모델 커스터마이징이 제한적이다.
- 정량적인 결과물 평가가 어렵다.

### 13.3 LLM 평가하기
- 정량적 지표들이 있지만, 질적인 평가에 한계가 있고, 절대적 잣대가 될 수 없다.
- `벤치마크 데이터셋`: 모델의 성능을 비교하기 위해 공통으로 사용하는 데이터셋
- 사람이 생성 결과를 직접 평가하는 방법을 사용하기도 한다.
- LLM을 통한 평가: '사람의 요청에 얼마나 잘 대응하는지' 확인하기 위해 LLM으로 평가.
  - 사람과 LLM의 평가가 80% 이상 일치하다는 연구 결과가 있음.


## 4부. 멀티 모달, 에이전트 그리고 LLM의 미래
## 14. 멀티 모달 LLM
### 14.1 멀티 모달 LLM이란
- 멀티 모달 LLM이란, 텍스트뿐만 아니라 이미지, 비디오, 오디오, 3D 등 다양한 형식의 데이터를 이해하고 생성할 수 있는 LLM을 말한다.
- 멀티 모달 LLM은 뛰어난 언어 이해 능력과 추론 능력을 중심으로 다양한 형식의 데이터를 이해하고 생성하는 능력을 추가하는 방식으로 구현된다.
- 이름에서 알 수 있듯이, LLM이 가장 핵심적인 역할을 한다. LLM의 뛰어난 이해 능력과 추론 능력을 활용한다.
- 멀티 모달 LLM을 만들기 위해서는 사전 학습된 텍스트 전용 LLM이 멀티 모달 입력과 출력을 지원하기 위한 추가 훈련 단계를 거친다.
- 멀티 모달 LLM의 학습 과정 = 사전 학습 + 지시 데이셋을 활용한 지시 학습
  - 사전 학습 단계에서 LLM은 이미지-텍스트 쌍과 같은 대규모 멀티 모달 데이터 세트로 학습한다.
  - 사전 학습 후 , 멀티 모달 지시 튜닝 단계에서 멀티 모달 지시 데이터셋으로 미세 조정한다.

### 14.2 이미지와 텍스트를 연결하는 모델: CLIP
- CLIP(Contrastive Language-Image Pre-Training)
- 텍스트 데이터와 이미지 데이터의 관계를 계산할 수 있도록 텍스트 모델과 이미지 모델을 함께 학습시킨 모델
- 서로 관련있는 이미지와 텍스트 쌍을 활용해 학습시킨다. (이미지 + 이미지에 대한 설명)
- 유사한 데이터 쌍은 더 가깝게, 유사하지 않은 데이터 쌍은 더 멀게 학습시킨다.
- 일반적으로는 뛰어난 성능을 보여주나, 위성 사진/자율 주행 관련 데이터 등 전문화되고 복잡한 데이터는 잘 작동하지 않는다.

### 14.3 텍스트로 이미지를 생성하는 모델: DALL-E
- DALL-E 2에서 사용하는 이미지 생성 모델: 디퓨전(diffusion) 모델
  - 확산 = 물질이 농도가 높은 곳에서 낮은 곳으로 이동하는 현상
  - 디퓨전 모델은 이미지에서 어떤 부분이 노이즈인지 예측하는 방식으로 학습
  - 완전한 노이즈 상태의 이미지에서 노이즈를 예측하고 예측된 노이즈를 제거하면서 의미가 있는 이미지를 생성한다.
### 14.4 LLaVA
- LLaVA 모델은 이미지를 인식하는 CLIP 모델과 LLM을 결합해 모델이 이미지를 인식하고 그 이미지에 대한 텍스트를 생성할 수 있다.
#### 14.4.1 LLaVA의 학습 데이터
- 멀티 모달 대화 모델을 만들 때 가장 어려운 부분은 대부분의 딥러닝 학습과 마찬가지로, 데이터셋이 부족하다는 점이다.
  - GPT-4는 입력 부분에 이미지와 이미지를 설명하는 캡션, 이미지에 어떤 물체가 있는지 물체와 위치정보를 나타내는 데이터셋을 활용했다.

### 14.5 정리
- 시장의 관심은 더 다양한 데이터를 처리할 수 있는 멀티 모달 LLM으로 빠르게 옮겨가고 있다.


## 15장. LLM 에이전트
- 에이전트(Agent): 알아서 생각하고 행동하는 시스템
- 에이전트는 LLM과 RAG을 하나의 구성요소로 사용
- 에이전트의 성능 평가가 아직까지는 정답이 없다.

### 15.1 에이전트란
- AI 분야에서 에이전트는 "주변 환경을 감각을 통해 인식하고 의사결정을 내려 행동하는 인공적인 개체"
#### 15.1.1 에이전트의 구성요소
- 먼저 감각(perception)을 통해 외부 환경과 사용자의 요청을 인식한다.
- 두뇌(brain)를 통해 가지고 있는 지식이나 지금까지의 기억을 확인해 보고 계획을 세우거나 추론을 통해 다음에 어떤 행동을 해야 할지 의사결정을 내린다.
- 문제를 해결하기 위해 취할 수 있는 적절한 도구를 선택해 행동(action)한다.
#### 15.1.2 에이전트의 두뇌
- 에이전트의 핵심은 생각하고 판단하는 두뇌다.
- 사용자 요청을 인식하고, 사용자의 대화를 확인해 상황을 이해하는 데 필요한 지식을 검색하고, 목표 달성을 위해 작업을 세분화하는 계획 세우기 단계를 거치고 행동 단계로 넘어간다.
- 현재까지 판단 내용을 기록하거나 유용한 지식을 저장하려면 효율적으로 요약하는 기능도 필요한데, LLM은 이 요약도 훌륭히 수행한다.
#### 15.1.4 에이전트의 행동
- LLM은 텍스트만 생성할 수 있다. 따라서 외부에 영향을 미치는 행동을 하기 위해서는 LLM이 사용할 수 있는 도구를 제공해야 한다.
  - 특정 도구를 어떨 때 사용하고 어떻게 사용하는지 설명을 제공하면, LLM은 상황에 맞춰 필요한 도구를 선택할 수 있다.

### 15.2 에이전트 시스템의 형태
- 단일 에이전트: 사용자가 입력한 목적을 달성하기 위해 단일 에이전트가 필요한 작업을 계획하고 행동하는 방식
- 멀티 에이전트: 여러 에이전트가 협력해서 목표를 달성하는 방식
#### 15.2.1 단일 에이전트
- AutoGPT의 프롬프트
  - 에이전트의 이름과 역할을 부여
  - 목표를 설정
  - 제약사항 전달
  - 사용 가능한 도구 목록 제공
  - 성능 평가(에이전트가 지속적으로 목표를 달성하는 데 초점을 맞추고 자원을 효율적으로 사용하도록 명령)
  - 답변 형태를 명시
- 목표가 명확하고 작업의 크기가 작은 경우에만 선택적으로 활용하는 편이 좋다.
  - 모든 과정을 스스로 처리하기 때문에 편하면서도, 답변 품질이 떨어질 수 있다.
- 특정 도메인에서 LLM의 전문성이 사람에 비해 떨어지는 경우에는 사용자가 문제 해결 과정에 직접 개입하는 것이 특히 중요하다.
  - 명확한 명령과 피드백으로 에이전트의 작업 방향을 설정하고, 에이전트는 작은 작업을 자동으로 수행해서 사용자가 빠르고 편하게 작업을 진행하도록 지원할 수 있다.
#### 15.2.3 멀티 에이전트
- 하나의 역할을 가진 에이전트가 모든 일을 수행하는 단일 에이전트와 달리 각 에이전트마다 서로 다른 프로필을 주고 작업을 수행한다.
  - 구체적인 프로필을 부여하는 것만으로도 관련된 작업의 전문성을 높여 결과 품질을 높일 수 있다.
  - LLM은 큰 덩어리의 작업보다는 구체적이고 세분화된 작업을 잘 수행하고 역할이 부여되면 성능이 더 높아진다.

### 15.3 에이전트 평가하기
- 평가 방식을 결정하고 개발해야 길을 잃지 않을 수 있다.
  - 전통적인 소프트웨어와는 다르게 입력에 따라 정해진 답이 나오지 않기 때문이다.
- 에이전트의 평가방식은 사람이 평가하는 주관적인 방식과, 테스트 데이터로 평가하는 객관적인 방식으로 나눌 수 있다.
- 에이전트 평가 기준
  1. 유용성(작업 성공률 등)
  2. 사회성(언어 숙련도, 역할 부합 정도 등)
  3. 가치관(신뢰성, 비차별적 등)
  4. 진화 능력(상황에 맞춰 스스로 발전)

## 16장. 새로운 아키텍처
- 2017년에 발표한 트랜스포머 아키텍처가 오랫동안 대세
- 2023년 말에 발표함 맘바(Mamba) 아키텍처가 트랜스포머보다 성능이 비슷하거나 뛰어나면서 추론 속도가 5배라고 주장
  - RNN(Recurrent Neural Network)을 개선한 모델
  - SSM(State Space Model)과 선택 메커니즘(selective mechanism)을 활용

### 16.1 기존 아키텍처의 장단점
- RNN 추론
  - 효율적이지만 학습할 때 입력을 병렬로 처리하지 못하고 순차적으로 입력하기 때문에 학습 속도가 느리다.
  - 모델 업데이트를 위한 학습 시 불안정함.
  - 한정된 메모리에 맥락을 압축하는 RNN의 특성상 문장이 길어지면 성능이 떨어짐.
- 트랜스포머
  - 어텐션 연산 사용. 시퀀스 길이가 길어져도 성능이 잘 유지됨.
  - 이전까지의 모든 텍스트와 관련도를 계산하기 때문에 매 순간 모든 텍스트를 참조하여 상당히 무거움.
  - 병렬화가 가능함. 학습 속도가 빠르고 어텐션 연산으로 RNN보다 성능이 높음.
  - 시퀀스 길이가 길어지면 빠르게 연산량이 커짐.

### 16.2 SSM
- SSM
  - 내부 상태를 가지고 시간에 따라 달라지는 시스템을 해석하기 위해 사용하는 모델링 방법
  - 학습과 추론에서 효율적이면서 긴 시퀀스 입력에 뛰어난 성능을 보인다.
  - 오디오, 시계열 데이터와 같이 텍스트에 비해 입력이 긴 사용 사례에서도 S4는 뛰어난 성능을 보인다.
  - 하지만 언어 모델링처럼 이산적이고 정보 집약적인 작업에서는 트랜스포머 보다 낮은 성능을 보임.
    - 이를 개선하기 위해 선택 메커니즘을 도입.
