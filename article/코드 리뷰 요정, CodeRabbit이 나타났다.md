# [코드 리뷰 요정, CodeRabbit이 나타났다 🐰](https://tech.inflab.com/20250303-introduce-coderabbit/)

### [Code Rabbit](https://docs.coderabbit.ai/) : 코드를 리뷰하는 AI
### 기능
1. 코드 변경사항 요약
    - workthrough
    - 시퀀스 다이어그램
2. 코드 리뷰 및 피드백
    - 오타
    - 함수 인자가 잘못된 경우 (자바에서 유용)
    - 디버깅을 위한 코드를 남긴 경우
    - 팀 컨벤션에 어긋나는 경우
3. 코드 질문 및 이슈 생성
    - 코드 제안
    - 이슈 생성
4. 컨벤션 설정 (Review Instructions)
    - AI에게 코드 리뷰 가이드라인 지정
    - `.coderabbit.yaml`파일에 프로젝트별로 설정 가능
5. 코드리뷰 리포트 (종합)

### 정적 분석도구와의 차이
- 코드의 패턴 분석에 치중한 정적 분석은 코드의 맥락과 의도까지 파악하기 어렵다.
- CodeRabbit은 PR에 포함된 코드 변경 내용을 읽고 판단하여 리뷰한다.
