# LangChain OpenTutorial - Windows 환경 설정 가이드

> 이 가이드는 Windows 환경에서 LangChain OpenTutorial 프로젝트를 위한 개발 환경을 설정하는 과정을 단계별로 설명합니다.

## 📋 목차

- [개요](#개요)
- [설치된 도구 및 버전](#설치된-도구-및-버전)
- [단계별 설정 과정](#단계별-설정-과정)
  - [1. Git 설치](#1-git-설치)
  - [2. PowerShell 정책 설정](#2-powershell-정책-설정)
  - [3. pyenv 설치](#3-pyenv-설치)
  - [4. Python 3.11 설치](#4-python-311-설치)
  - [5. Poetry 설치](#5-poetry-설치)
  - [6. 프로젝트 패키지 설치](#6-프로젝트-패키지-설치)
  - [7. VS Code 설정](#7-vs-code-설정)
- [주요 개념 이해하기](#주요-개념-이해하기)
- [트러블슈팅](#트러블슈팅)
- [최종 환경 정보](#최종-환경-정보)
- [다음 단계](#다음-단계)

---

## 개요

이 프로젝트는 **pyenv + Poetry** 조합으로 환경을 관리합니다:
- **pyenv**: Python 버전 관리
- **Poetry**: 의존성 및 가상환경 관리
- **VS Code**: 개발 환경 (Jupyter Notebook 지원)

---

## 설치된 도구 및 버전

| 도구 | 버전 | 역할 |
|------|------|------|
| Git | 2.51.0.windows.1 | 버전 관리 |
| pyenv | 3.1.1 | Python 버전 관리 |
| Python | 3.11.9 | 프로그래밍 언어 |
| Poetry | 2.2.1 | 패키지 및 가상환경 관리 |
| VS Code | - | 통합 개발 환경 |

---

## 단계별 설정 과정

### 1. Git 설치

**다운로드:**
- [Git for Windows](https://git-scm.com/download/win)

**설치 확인:**
```powershell
git --version
# 출력: git version 2.51.0.windows.1
```

---

### 2. PowerShell 정책 설정

pyenv 설치를 위해 PowerShell 실행 정책을 변경합니다.

**PowerShell을 관리자 권한으로 실행:**
```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
```

---

### 3. pyenv 설치

#### 3-1. pyenv 다운로드
```powershell
git clone https://github.com/pyenv-win/pyenv-win.git "$env:USERPROFILE\.pyenv"
```

#### 3-2. 환경변수 설정
```powershell
# pyenv 환경변수 설정
[System.Environment]::SetEnvironmentVariable('PYENV', $env:USERPROFILE + "\.pyenv\pyenv-win\", "User")
[System.Environment]::SetEnvironmentVariable('PYENV_ROOT', $env:USERPROFILE + "\.pyenv\pyenv-win\", "User")
[System.Environment]::SetEnvironmentVariable('PYENV_HOME', $env:USERPROFILE + "\.pyenv\pyenv-win\", "User")

# PATH 설정
[System.Environment]::SetEnvironmentVariable('PATH', $env:USERPROFILE + "\.pyenv\pyenv-win\bin;" + $env:USERPROFILE + "\.pyenv\pyenv-win\shims;" + [System.Environment]::GetEnvironmentVariable('PATH', "User"), "User")
```

#### 3-3. PowerShell 재시작 후 확인
```powershell
pyenv --version
# 출력: pyenv 3.1.1
```

**주의:** PowerShell을 재시작해야 환경변수가 적용됩니다.

---

### 4. Python 3.11 설치

#### 4-1. Python 3.11 설치
```powershell
pyenv install 3.11
```

#### 4-2. 프로젝트에만 Python 3.11 적용
```powershell
cd "C:\Python Projects\LangChain-OpenTutorial"
pyenv local 3.11.9
```

이렇게 하면 이 프로젝트 폴더에서만 Python 3.11.9를 사용하고, 다른 프로젝트는 영향받지 않습니다.

#### 4-3. 확인
```powershell
python --version
# 출력: Python 3.11.9
```

---

### 5. Poetry 설치

#### 5-1. Poetry 설치
```powershell
pip install poetry
```

#### 5-2. 확인
```powershell
poetry --version
# 출력: Poetry (version 2.2.1)
```

---

### 6. 프로젝트 패키지 설치

#### 6-1. 의존성 설치
```powershell
cd "C:\Python Projects\LangChain-OpenTutorial"
poetry install
```

이 명령어는:
- 자동으로 가상환경 생성
- `pyproject.toml`에 명시된 모든 패키지 설치
- `poetry.lock`에 정확한 버전 기록

**주의:** 패키지가 많아서 5-15분 정도 걸릴 수 있습니다.

#### 6-2. 가상환경 정보 확인
```powershell
poetry env info
```

#### 6-3. 가상환경 경로 확인
```powershell
poetry env info --path
# 출력 예: C:\Users\USER\AppData\Local\pypoetry\Cache\virtualenvs\langchain-opentutorial-xxxxx-py3.11
```

---

### 7. VS Code 설정

#### 7-1. 필요한 확장 설치
- **Python** (Microsoft)
- **Jupyter** (Microsoft)

#### 7-2. 프로젝트 폴더 열기
```
File → Open Folder → C:\Python Projects\LangChain-OpenTutorial
```

#### 7-3. Python 인터프리터 선택

**방법 1: Jupyter Notebook에서**
1. `.ipynb` 파일 열기
2. 우측 상단 "Select Kernel" 클릭
3. "Python Environments" 선택
4. Poetry 가상환경 선택 (`langchain-opentutorial-...`)

**방법 2: 수동으로 경로 지정**
1. `Ctrl+Shift+P`
2. "Python: Select Interpreter" 입력
3. "Enter interpreter path..." 선택
4. Poetry 가상환경 경로의 `Scripts\python.exe` 입력
   ```
   C:\Users\USER\AppData\Local\pypoetry\Cache\virtualenvs\langchain-opentutorial-xxxxx-py3.11\Scripts\python.exe
   ```

#### 7-4. 테스트
```python
import langchain
print(f"LangChain version: {langchain.__version__}")
# 출력: LangChain version: 0.3.13
```

---

## 주요 개념 이해하기

### pyenv vs venv vs Poetry

#### **pyenv**
- **역할:** Python 버전 관리자
- **가상환경 X**
- 여러 Python 버전(3.9, 3.11, 3.12 등)을 설치하고 전환

```
C:\Users\USER\.pyenv\versions\
├── 3.9.13\
├── 3.11.9\  ← 우리가 사용 중
└── 3.12.1\
```

#### **venv (표준 라이브러리)**
- **역할:** 가상환경 생성
- Python 표준 도구
- 수동으로 관리

```bash
python -m venv myenv
myenv\Scripts\activate
pip install langchain
```

#### **Poetry 가상환경**
- **역할:** venv와 동일하지만 Poetry가 자동 관리
- 의존성 관리 포함
- 더 현대적인 방식

```bash
poetry install  # 자동으로 가상환경 생성 + 패키지 설치
poetry run python script.py
```

---

### pyproject.toml vs poetry.lock vs requirements.txt

#### **requirements.txt (전통적 방식)**
```txt
langchain==0.3.13
pandas>=2.2.3
```

**문제점:**
- 의존성의 의존성을 관리 못함
- 재현성 부족 (`>=`는 매번 다른 버전 설치 가능)

#### **pyproject.toml (Poetry 설정 파일)**
- **사람이 작성**
- "원하는 패키지와 버전 범위" 선언
- 프로젝트 메타데이터 포함

```toml
[tool.poetry.dependencies]
python = ">=3.10,<3.13"
langchain = "0.3.13"
pandas = ">=2.2.3"
```

#### **poetry.lock (Poetry 잠금 파일)**
- **Poetry가 자동 생성**
- "실제로 설치된 정확한 버전" 기록
- 모든 의존성(의존성의 의존성 포함)을 재귀적으로 기록
- 100% 재현 가능한 환경 보장

**비유:**
- `pyproject.toml` = 레시피 ("토마토 2개, 양파 적당량")
- `poetry.lock` = 정확한 재료 목록 ("토마토 2개 153g, 양파 1개 87g")

---

### 가상환경과 인터프리터의 관계

**가상환경 = 특정 폴더 안의 python.exe + 설치된 패키지들**

```
Poetry 가상환경:
C:\...\langchain-opentutorial-xxxxx-py3.11\
├── Scripts\
│   ├── python.exe     ← 이것이 인터프리터
│   ├── pip.exe
│   └── jupyter.exe
└── Lib\
    └── site-packages\
        ├── langchain\
        ├── openai\
        └── ...
```

**인터프리터 선택 = 어떤 python.exe를 사용할지 지정**

VS Code에서 Poetry 가상환경의 `python.exe`를 선택하면:
= 가상환경 사용
= venv 활성화와 동일한 효과

---

## 트러블슈팅

### 문제 1: python 명령어가 인식되지 않음

**증상:**
```powershell
python --version
# 에러: 'python' 용어가 cmdlet, 함수, 스크립트 파일 또는 실행할 수 있는 프로그램 이름으로 인식되지 않습니다.
```

**원인:** Microsoft Store의 Python 앱 별칭이 pyenv보다 우선순위가 높음

**해결:**
1. Windows 설정 열기 (`Win + I`)
2. 앱 → 고급 앱 설정 → 앱 실행 별칭
3. `python.exe` 와 `python3.exe` 를 **끄기**로 변경
4. PowerShell 재시작

---

### 문제 2: PowerShell 재시작 시 pyenv/python 인식 안 됨

**증상:** PowerShell을 새로 열면 pyenv가 작동하지 않음

**해결:** PowerShell 프로필에 자동 로드 설정

1. PowerShell 프로필 열기:
```powershell
notepad $PROFILE
```

2. 다음 내용 추가:
```powershell
$env:PYENV = "$env:USERPROFILE\.pyenv\pyenv-win\"
$env:PYENV_ROOT = "$env:USERPROFILE\.pyenv\pyenv-win\"
$env:PYENV_HOME = "$env:USERPROFILE\.pyenv\pyenv-win\"
$env:PATH = "$env:USERPROFILE\.pyenv\pyenv-win\bin;$env:USERPROFILE\.pyenv\pyenv-win\shims;$env:PATH"
```

3. 저장 후 PowerShell 재시작

---

### 문제 3: VS Code에서 Poetry 가상환경이 목록에 안 보임

**해결:**

**방법 1:** 수동으로 경로 지정
1. `Ctrl+Shift+P`
2. "Python: Select Interpreter"
3. "Enter interpreter path..."
4. `poetry env info --path` 결과의 `\Scripts\python.exe` 입력

**방법 2:** 프로젝트 내부에 가상환경 생성
```powershell
poetry config virtualenvs.in-project true
poetry env remove python
poetry install
```
이렇게 하면 프로젝트 폴더에 `.venv` 폴더가 생성되고 VS Code가 자동 인식합니다.

---

## 최종 환경 정보

### 프로젝트 구조
```
LangChain-OpenTutorial/
├── .python-version          # pyenv local 설정 (3.11.9)
├── pyproject.toml           # Poetry 설정 (의존성 선언)
├── poetry.lock              # 정확한 버전 잠금
├── .env                     # API 키 (Git에 커밋하지 말 것!)
├── .gitignore              # .env 포함
├── 01-Basic/               # 기본 튜토리얼
├── 02-Prompt/              # 프롬프트 관련
└── ...
```

### Python 환경
```
Python: 3.11.9 (pyenv 관리)
위치: C:\Users\USER\.pyenv\versions\3.11.9\
```

### 가상환경
```
관리자: Poetry
위치: C:\Users\USER\AppData\Local\pypoetry\Cache\virtualenvs\langchain-opentutorial-xxxxx-py3.11\
주요 패키지:
  - langchain 0.3.13
  - langgraph 0.2.60
  - langchain-openai 0.2.14
  - jupyter 1.1.1
  - 기타 100+ 패키지
```

---

## 다음 단계

### 1. API 키 설정

#### OpenAI API 키 발급
- [OpenAI Platform](https://platform.openai.com/)에서 API 키 생성
- 참고: `01-Basic/03-OpenAIAPI-Key-Generation.ipynb`

#### .env 파일 생성
프로젝트 루트에 `.env` 파일을 만들고:
```env
OPENAI_API_KEY=sk-...your-key...
```

**주의:** `.env` 파일은 절대 Git에 커밋하지 마세요!

### 2. LangSmith 설정 (선택사항)
- LangChain 앱 모니터링 및 디버깅
- 참고: `01-Basic/04-LangSmith-Tracking-Setup.ipynb`

### 3. 튜토리얼 시작
- `01-Basic/` 폴더부터 시작
- Jupyter Notebook (`.ipynb`) 형식으로 제공

---

## 유용한 명령어

### Poetry 명령어
```powershell
# 가상환경 정보 확인
poetry env info

# 가상환경 경로 확인
poetry env info --path

# 패키지 추가
poetry add requests

# 패키지 제거
poetry remove requests

# 의존성 업데이트
poetry update

# 가상환경에서 명령 실행
poetry run python script.py
poetry run jupyter notebook

# 설치된 패키지 목록
poetry show
```

### pyenv 명령어
```powershell
# 설치된 Python 버전 목록
pyenv versions

# 사용 가능한 Python 버전 목록
pyenv install --list

# 전역 Python 버전 설정
pyenv global 3.11

# 로컬(프로젝트) Python 버전 설정
pyenv local 3.11.9

# 현재 Python 버전 확인
pyenv version
```

---

## 참고 자료

- [LangChain OpenTutorial GitHub](https://github.com/LangChain-OpenTutorial/LangChain-OpenTutorial)
- [Poetry 공식 문서](https://python-poetry.org/docs/)
- [pyenv-win GitHub](https://github.com/pyenv-win/pyenv-win)
- [LangChain 공식 문서](https://python.langchain.com/docs/introduction/)

---

