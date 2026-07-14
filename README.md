# AI 엔지니어링 실습

프롬프트 엔지니어링부터 하네스 엔지니어링까지 — 직접 해보는 실습 자료입니다.

| 실습 | 내용 | 환경 | 자료 |
|------|------|------|------|
| 실습 1 | 프롬프트 스코어링 — 프롬프트를 고쳐서 점수를 올려보자 | **Google Colab** (설치 불필요) | [01_prompt_engineering.ipynb](01_prompt_engineering.ipynb) |
| 실습 2 | 나만의 에이전트 만들기 — CLAUDE.md로 에이전트 행동 바꾸기 | 내 컴퓨터 (로컬) | [02_harness_engineering/](02_harness_engineering/) |


---

## 실습 1 시작하기 (Colab — 브라우저만 있으면 됨)

Google 계정만 있으면 됩니다. 아무것도 설치하지 않습니다.

1. https://colab.research.google.com 접속
2. **파일 → 노트북 업로드** 로 `01_prompt_engineering.ipynb` 업로드
   (GitHub에 올라가 있다면: **GitHub 탭**에서 `ybkim453/AI-engineering` 검색 후 열기)
3. 노트북 안의 안내를 따라 위에서부터 순서대로 셀 실행

> 첫 준비 셀 2개가 AI 모델을 Colab에 설치합니다 (약 2~3분). 그 다음부터는 빠릅니다.

---

## 실습 2를 위한 로컬 환경 설정

실습 2는 **내 컴퓨터에 직접** AI 에이전트를 설치합니다. 아래 순서대로 따라 하세요.

### 0. VS Code 설치

코드를 편집하고 터미널을 사용할 프로그램입니다.

- https://code.visualstudio.com/ 에서 다운로드 후 설치
- 설치 후 VS Code를 열고, 상단 메뉴 **Terminal → New Terminal** 로 터미널을 엽니다
- 앞으로 나오는 모든 명령어는 이 터미널에 입력합니다

### 1. Python 설치

터미널에 아래 명령어를 입력합니다:

```powershell
winget install Python.Python.3.11
```

설치가 끝나면 **터미널을 완전히 껐다가 다시 켜고** 확인합니다:

```powershell
python --version
```

`Python 3.11.x` 처럼 나오면 성공입니다.

> `winget`이 없다고 나오면 https://www.python.org/downloads/ 에서 설치 파일을 받으세요.
> 설치 화면에서 **"Add python.exe to PATH" 체크박스를 반드시 체크**해야 합니다!

### 2. Ollama 설치 (무료 로컬 AI)

Ollama는 AI 모델을 내 컴퓨터에서 무료로 실행해주는 프로그램입니다.

```powershell
irm https://ollama.com/install.ps1 | iex
```

설치 후 터미널을 다시 켜고, AI 모델을 다운로드합니다 (약 1.4GB):

```powershell
ollama pull qwen3:1.7b
```

잘 되는지 테스트:

```powershell
ollama run qwen3:1.7b "안녕하세요"
```

AI가 대답하면 성공입니다. `/bye` 를 입력하면 대화가 종료됩니다.

### 3. 실습 폴더 열기

VS Code에서 **File → Open Folder** 로 이 `실습자료` 폴더를 엽니다.

### 4. 가상환경(venv) 만들기

가상환경은 "이 프로젝트 전용 파이썬 공간"입니다.
컴퓨터 전체를 어지럽히지 않고 필요한 패키지를 깔 수 있습니다.

터미널에서 (이 `실습자료` 폴더 위치에서):

```powershell
python -m venv harness_env
```

가상환경 켜기:

```powershell
harness_env\Scripts\activate
```

터미널 앞에 `(harness_env)` 가 붙으면 성공입니다.

> **오류가 난다면?** "스크립트를 실행할 수 없습니다" 라고 나오면 아래 명령어를 먼저 실행하세요:
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
> 그 다음 다시 `harness_env\Scripts\activate` 를 실행합니다.

> **주의**: 터미널을 새로 열 때마다 `harness_env\Scripts\activate` 를 다시 실행해야 합니다.

---

## 로컬 환경 설정 완료 체크리스트 (실습 2용)

- [ ] VS Code 설치됨
- [ ] `python --version` → 3.11.x 출력
- [ ] `ollama run qwen3:1.7b "안녕"` → AI가 대답함
- [ ] 터미널 앞에 `(harness_env)` 표시됨

전부 체크했다면 [02_harness_engineering/](02_harness_engineering/) 으로 이동하세요!
