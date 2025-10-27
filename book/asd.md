# [클로드 코드 완벽 가이드](https://search.shopping.naver.com/book/catalog/56356122391?cat_id=50010921&frm=PBOKPRO&query=%ED%81%B4%EB%A1%9C%EB%93%9C+%EC%BD%94%EB%93%9C+%EC%99%84%EB%B2%BD+%EA%B0%80%EC%9D%B4%EB%93%9C&NaPm=ct%3Dmh976i0g%7Cci%3D4526ed68f8b619f09279e19ac8529879d509a197%7Ctr%3Dboknx%7Csn%3D95694%7Chk%3D477ff57cc7249466912daa44500cc06d56c87444)
- 지은이: 최지호
- 출판사: 골든래빗
- 읽은 날짜: 2025.10.21 ~ 


## 챕터 05. 슬래시 명령어 제대로 알아보기
- `/model`: 기본값은 자동으로 클로드 모델을 바꿔준다. 바뀌는 기준은 비용.
- `/compact`: 컨텍스트를 압축. 컨텍스트 토큰이 200K가 넘어가면 자동으로 compact가 실행됨.
- `/config`
  - Use todo list 옵션 켜기.
- `/memory`: CLAUDE.MD가 메모리 역할을 함. 이 파일을 통해 메모리 수정 가능.

## 챕터 06. CLAUDE.md 파일에 대한 모든 것
- CLAUDE.md는 클로드 코드의 효율에 큰 영향을 준다.
  - 맞춤형 지침 파일이다.
- 최대한 필요한 내용만 간추리는 것이 좋다.
  - 어쨌든 컨텍스트 윈도우의 한계가 있기 때문이다.
- 용도 정리
  - `~/.claude/CLAUDE.md` 사용자 전역 설정
  - `./CLAUDE.md` 프로젝트 메모리
  - `./claude.local.md` 프로젝트 로컬 메모리 (gitignore 설정 필수)
- CLI 명령을 `#`로 시작하면 CLAUDE.md에 프롬프트를 반영한다.
- 작업 내용을 CLAUDE.md에 반영을 주기적으로 요청하고, CLAUDE.md 자체 정리도 주기적으로 요청한다.
- CLAUDE.md 파일도 커밋할 것. (클로드코드를 통해 일관성 있는 코드를 생성하기 위함.)
