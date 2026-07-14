# 문제 해결 가이드

실습 중 자주 발생하는 문제와 해결법입니다.

---

## 설치 관련

### `winget`을 찾을 수 없다고 나옴
- Windows가 구버전일 수 있습니다. 수동 설치로 진행하세요:
  - Python: https://www.python.org/downloads/ → 설치 시 **"Add python.exe to PATH" 체크 필수**
  - Ollama: `irm https://ollama.com/install.ps1 | iex` (winget 불필요) 또는 https://ollama.com/download

### `python`을 입력하면 Microsoft Store가 열림
- 설치 후 터미널을 껐다 켜지 않아서 그렇습니다. VS Code를 완전히 재시작하세요.
- 그래도 안 되면: Windows 설정 → 앱 → 고급 앱 설정 → 앱 실행 별칭 → `python.exe` 항목 **끄기**

### `harness_env\Scripts\activate` 실행 시 "스크립트를 실행할 수 없습니다"
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
실행 후 다시 activate 하세요.

---

## Ollama 관련

### `ollama pull`이 너무 느림
- 모델(약 1.4GB) 다운로드는 인터넷 속도에 따라 몇 분 걸릴 수 있습니다. 기다리세요.
- 여러 명이 동시에 받으면 교실 와이파이가 느려집니다 → 순서대로 받거나 미리 받아두세요.

### "Ollama에 연결할 수 없습니다" 오류
1. Ollama가 실행 중인지 확인 — 작업 표시줄 트레이에 라마 아이콘이 있어야 합니다
2. 없다면 시작 메뉴에서 Ollama 실행, 또는 터미널에서:
   ```powershell
   ollama serve
   ```
3. 모델을 받았는지 확인:
   ```powershell
   ollama list
   ```
   `qwen3:1.7b` 가 목록에 없으면 `ollama pull qwen3:1.7b`

### 모델 응답이 너무 느림
- 첫 질문은 모델을 메모리에 올리느라 오래 걸립니다 (10~30초). 두 번째부터 빨라집니다.
- 계속 느리면 다른 무거운 프로그램(게임, 브라우저 탭 수십 개)을 닫으세요.

---

## OpenHarness 관련

### `openh`를 찾을 수 없다고 나옴
1. 가상환경이 켜져 있나요? 터미널 앞에 `(harness_env)` 가 있어야 합니다:
   ```powershell
   harness_env\Scripts\activate
   ```
2. 설치가 됐나요?
   ```powershell
   pip install openharness-ai
   ```

### `oh` 명령어가 이상하게 동작함
- Windows에서 `oh`는 PowerShell 기본 명령어(`Out-Host`)와 충돌합니다.
- **반드시 `openh` 를 사용하세요.**

### "Error: No API key configured" 라고 나옴
- provider 등록 시 `--api-key ollama` 를 빼먹은 것입니다. 아래 한 줄로 해결:
  ```powershell
  openh provider edit ollama --api-key ollama
  ```
- Ollama는 진짜 키가 필요 없지만, 프로그램이 키가 하나는 있어야 시작되기 때문에 더미 값을 넣는 것입니다.

### 에이전트가 응답을 안 하거나 오류가 남
1. provider 설정 확인:
   ```powershell
   openh provider list
   ```
   `ollama` 항목이 있고 활성화(`use`) 되어 있어야 합니다.
2. 없다면 README의 2단계 명령어를 다시 실행하세요.
3. Ollama가 켜져 있는지 확인 (`ollama list` 가 동작하는지)

### 에이전트가 "그런 도구가 없다"고 함 (폴더 확인, 파일 생성 등)
- Ollama 컨텍스트 길이(기본 4,096토큰)가 부족해서 **도구 목록이 잘린 것**입니다.
- 해결:
  ```powershell
  setx OLLAMA_CONTEXT_LENGTH 32768
  ```
- 실행 후 Ollama 재시작 필수: 트레이의 라마 아이콘 우클릭 → Quit Ollama → 시작 메뉴에서 재실행

### 에이전트가 CLAUDE.md 규칙을 무시함
- CLAUDE.md 파일이 **에이전트를 실행한 폴더**(예: `my-agent/`)에 있는지 확인하세요.
- 파일 이름이 정확히 `CLAUDE.md` 인지 확인 (`claude.md.txt` 처럼 되어 있지 않은지)
- CLAUDE.md를 수정했다면 에이전트를 껐다가 다시 실행해야 반영됩니다.
- 작은 모델(1.7b)이라 가끔 규칙을 어길 수 있습니다. 규칙을 더 짧고 명확하게 써보세요.

---

## 실습 1 (Colab 프롬프트 스코어링) 관련

### "[오류] 프롬프트 안에 {문제} 가 없습니다"
- 프롬프트를 수정하다가 `{문제}` 를 지웠습니다. 다시 넣어주세요.
- `{문제}` 자리에 채점용 문제가 자동으로 들어가는 구조입니다.

### 점수가 계속 0점
- 모델이 답변 형식을 안 지키는 것입니다. 프롬프트에 형식을 명확히 지정하세요:
  - 예: "긍정, 부정, 중립 중 **하나의 단어로만** 답해"
- 모델이 중국어나 영어로 답하면: "한국어로만 답해" 를 추가하세요.

### Colab에서 "urlopen error" 또는 연결 오류
- 준비 셀 2개를 실행하지 않았거나, 런타임이 초기화된 것입니다.
- Colab은 오래 방치하면 런타임이 끊깁니다 → **준비 1, 준비 2 셀부터 다시 실행**하세요.

### Colab 셀 실행이 멈춰 있음
- 상단 메뉴 **런타임 → 세션 다시 시작** 후 준비 셀부터 다시 실행하세요.
