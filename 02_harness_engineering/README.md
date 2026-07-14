# 실습 2 — 나만의 에이전트 만들기

실습 1에서는 AI에게 **좋은 질문**을 만들었습니다.
이번에는 AI에게 **손과 규칙**을 줘서, 실제로 일하는 **에이전트**로 만들어봅니다.

사용 도구: **OpenHarness** (오픈소스 하네스) + **Ollama** (무료 로컬 모델)

> 시작 전에 [상위 폴더의 README](../README.md) 환경 설정(Python, Ollama, 가상환경)이 끝나 있어야 합니다.

---

## 1단계 — OpenHarness 설치

가상환경이 켜져 있는지 확인하세요 (터미널 앞에 `(harness_env)` 표시).
꺼져 있다면 `실습자료` 폴더에서:

```powershell
harness_env\Scripts\activate
```

설치:

```powershell
pip install openharness-ai
```

확인:

```powershell
openh --help
```

> **주의**: Windows에서는 `oh` 가 아니라 **`openh`** 를 사용하세요.
> (`oh`는 PowerShell의 기본 명령어 `Out-Host`와 충돌합니다)

## 2단계 — Ollama 연결

OpenHarness에게 "우리는 무료 로컬 모델을 쓸 거야"라고 알려주는 단계입니다.
아래 명령어를 **한 줄로** 입력하세요:

```powershell
openh provider add ollama --label "Ollama" --provider Ollama --api-format openai --auth-source openai_api_key --model qwen3:1.7b --base-url http://localhost:11434/v1 --api-key ollama
```

> 맨 뒤의 `--api-key ollama` 는 진짜 API 키가 아닙니다. Ollama는 키가 필요 없지만,
> 프로그램이 "키가 하나는 있어야" 시작되기 때문에 아무 값이나 넣어주는 것입니다.

그 다음 활성화:

```powershell
openh provider use ollama
```

마지막으로 **Ollama의 컨텍스트 길이를 늘려줍니다** (중요!):

```powershell
setx OLLAMA_CONTEXT_LENGTH 32768
```

실행 후 **Ollama를 완전히 재시작**하세요:
작업 표시줄 트레이의 라마 아이콘 우클릭 → **Quit Ollama** → 시작 메뉴에서 Ollama 다시 실행.

> **왜 필요한가?** Ollama는 기본적으로 4,096토큰까지만 읽습니다.
> 그런데 에이전트의 도구 43개 설명만으로도 이걸 넘어서, 도구 목록이 잘린 채 모델에게 전달됩니다.
> 그러면 에이전트가 "폴더를 볼 도구가 없다"는 식으로 착각하게 됩니다.

## 3단계 — 첫 실행 (CLAUDE.md 없이)

실습용 작업 폴더를 만들고 그 안에서 에이전트를 실행합니다:

```powershell
mkdir my-agent
cd my-agent
openh
```

에이전트가 켜지면 이것저것 시켜보세요:

- `안녕! 너는 누구야?`
- `블랙홀이 뭔지 설명해줘`
- `Explain what AI is` (영어로 물어보기)

**이때 에이전트가 어떻게 행동하는지 잘 관찰하세요.**
영어로 답하기도 하고, 답이 쓸데없이 길기도 하고, 말투도 그때그때 다릅니다.
→ 아직 **행동 규칙이 없기 때문**입니다.

> 파일 생성 같은 도구 사용은 작은 모델(1.7b)에서 불안정할 수 있습니다.
> 잘 되면 신기한 것, 안 되면 그것대로 배울 점 — 도전 미션에서 다룹니다.

종료: `/exit` 입력 (또는 Ctrl+C 두 번)

## 4단계 — CLAUDE.md 작성

`my-agent` 폴더 안에 `CLAUDE.md` 파일을 만드세요.
이 폴더의 [CLAUDE.md.example](CLAUDE.md.example) 을 참고해서 **여러분만의 규칙**을 적습니다.

예시:

```markdown
# CLAUDE.md

## 행동 원칙
- 항상 한국어로만 답변한다
- 파일을 만들거나 수정하기 전에 반드시 나에게 확인을 받는다
- 답변은 3문장 이내로 짧게 한다

## 역할
너는 고등학생의 공부를 도와주는 친절한 튜터다.
어려운 용어는 반드시 쉬운 말로 풀어서 설명한다.

## 금지 사항
- 영어로 답변하지 않는다
- 확인 없이 파일을 삭제하지 않는다
```

## 5단계 — 다시 실행하고 비교

```powershell
openh
```

3단계에서 했던 것과 **똑같은 명령**을 다시 시켜보세요.

| | CLAUDE.md 없을 때 | CLAUDE.md 있을 때 |
|---|---|---|
| 언어 | 영어로 물으면 영어로 답함 | 무조건 한국어로만 |
| 답변 길이 | 쓸데없이 김 | 내가 정한 길이로 |
| 말투/역할 | 그때그때 다름 | 내가 정한 역할대로 |

**같은 모델인데 행동이 달라졌다면 — 여러분은 방금 하네스 엔지니어링을 한 것입니다.**

---

## 미션

자세한 미션과 체크리스트는 [MISSION.md](MISSION.md) 를 보세요.

## 문제가 생겼다면

[TROUBLESHOOTING.md](TROUBLESHOOTING.md) 에서 해결법을 찾아보세요.
