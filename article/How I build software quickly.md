# [How I build software quickly](https://evanhahn.com/how-i-build-software-quickly/)

### 10점 만점에 8점을 목표로 하되, 제때 완료하라.
- 코드가 동작해야 한다.
- 약간의 문제가 있어도, 심각한 것은 없어야 한다.
- 그리고 시간을 엄수해야 한다.
- 프로젝트의 성격에 따라, 빠르게 만는데 집중할지, 코드 품질에 집중할지가 달라진다.

### 초안(Rough Draft)을 가능한 빨리 구현하라. (= 스켈레톤 코드)
- TODO가 난무하고, 코드가 엉망이더라도, 막연히라도 좋은 솔루션을 닮은 초안을 빠르게 구현하라.
- Rough Draft의 장점
  - 불필요한 조기 추상화를 방지할 수 있다.
  - 완료 시점을 더 정확히 예측할 수 있다.
  - demo를 시연하며 피드백을 받을 수 있다.
  - 알 수 없었던 문제를 알게되기도 한다.
  - 비가역적인 결정들(DB 스키마, 언어 선택 등)을 미리 해보고, 중대한 실수를 방지할 수 있다.
  - Input/Output을 먼저 만든다.

### 한 번에 큰 변화를 요구하기보다, 작고 점진적인 변화가 더 효과적이다.
- 작성이 쉽고, 리뷰가 쉽고, 되돌리기 쉽고, 버그를 더 발견하기 쉽다.
- 소스코드를 고치다가 길을 잃지 마라.
  - 해결 방법 1. 타이머 설정하기. (15분 ~ 1시간)
  - 해결 방법 2. 페어 프로그래밍

### 빠른 개발에 도움이 되는 유용한 스킬들
- 코드 읽기: 가장 중요한 개발자 역량
- 데이터 모델링: 시간이 걸려도 제대로 하는 것이 중요
- 스크립팅 (쉘, 파이썬 등)
- 디버거
- 휴식 타이밍 알기: 막힐 때는 과감히 휴식
- 순수 함수와 불변 데이터 선호
- LLM 사용하기

---
### Comment
- 평소와 비슷한 생각을 가지고 있어서(초안 구현하기, 작게 변화하기, 80점을 목표로 하기) 공감가는 글
- 타이머 설정해서 개발하는 건 집중력 높이기 좋은 것 같음.
- LLM으로 스크립팅 빠르게 짜면서 배워보기.
