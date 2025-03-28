# [나만의 Visual Studio Code Copilot 지침 만들고 활용하기](https://d2.naver.com/helloworld/6615449)

- [Copilot 공식 문서](https://code.visualstudio.com/docs/copilot/overview)
  - 제한된 무료 사용 가능
- Copilot의 도움을 받으려면 매번 프롬프트를 질의해야 하며, 환경에 맞게 답변하려면 프롬프트를 통해 매번 맥락을 전달해야 함
- 이러한 불편함을 없애기 위해 VS Code Copilot 지침을 만들기

### 커스텀 지침(Custom Instruction)
- 프로젝트 공통 지침은 `.github/copilot-instructions.md` 파일에 명시
  - 외부 리소르를 참조하도록 할 경우 제대로 작동하지 않을 수 있음.
  - 예시
``` 
	답변은 항상 한국어로 해줘.
	모든 코드는 python으로 작성해줘.
```

### VS Code 설정 파일(`settings.json`)에 명시하기
- `github.copilot.chat.{옵션}.instructions`
  - commitMessageGeneration
  - codeGeneration
  - testGeneration
  - reviewSelection
- 직접 프롬프트를 텍스트로 작성할 수도 있고, 다른 파일을 참조하게 할 수도 있음.

### 프롬프트 파일 사용하기
- VS Code 설정에서 `"chat.promptFiles": true` 설정
- `.github/prompts` 폴더에 `XXX.prompt.md` 파일 생성
- 해당 파일에 프롬프트 작성
  - 파일 내에서 다른 파일을 참조하게 만들 수도 있음.
