## 과제 목표

PR-Agent의 /algo 디렉터리는 기능의 중심, 핵심 알고리즘, 유틸리티 함수, 그리고 다양한 AI 모델과의 상호작용을 위한 핸들러를 포함하고 있습니다.  

/aglo 디렉터리 내부에서 다양한 문제점이 발견되었습니다.  

조원들이 분석하고 식별한 주요 문제점들  
(AI 핸들러 및 토큰 핸들러의 잠재적 비효율성, 일관성 없는 오류 처리, 특정 함수의 로깅 부족 등)을 해결하여 모듈의 전반적인 완성도를 높이는 것을 목표로 합니다.

## 과제 범위

- `AI Handler` (/ai_handlers)
  - 강경만, 서강문, 이승민
  - (`toekn_handler.py`, `Handler Blame`, `langchain_ai_handler.py`)

- `Utils` (/utils.py & /pr_processing.py)
  - 박성호, 손찬우, 장현상
  - (`load_yaml`, `utils.convert_to_markdown_v2`, `pr_processing.py`)  

### 세부 항목

- `toekn_handler.py`의 `count_tokens` 리팩토링
[Issue #1782](https://github.com/qodo-ai/pr-agent/issues/1782)

  - **내용**  
    - `count_tokens` 메소드가 `force_accurate` 플래그 처리, 모델 타입 확인, 모델별 계산 함수 라우팅 등을 직접 수행하는 구조를 개선하여,  
    향후 다양한 모델 지원 시 확장성과 유지보수성을 높임.  

  - **주요 작업**
    - `count_tokens`의 핵심 로직(기본 추정치 계산, `force_accurate` 분기)은 유지.
    - 모델별 정확한 토큰 수 계산 로직을 별도의 핸들러나 레지스트리 형태로 분리하여,  
    `determine_accurate_token_count()`와 같은 위임 메소드를 통해 호출하도록 구조 변경.  

  - **기대 효과**  
    이를 통해 `count_tokens` 메소드는 안정적으로 유지하고,  
    신규 모델 지원 시 모델별 계산 로직만 해당 위임 부분에 추가/수정하도록 구조를 개선합니다.  

#

- `LangChainOpenAIHandler` 견고성 및 인터페이스 준수 리팩토링
[Issue #1784](https://github.com/qodo-ai/pr-agent/issues/1784)

  - **내용**
    - `langchain` 라이브러리 미설치 시 `ImportError`가 숨겨져 런타임 `NameError`로 이어져 사용자 디버깅을 어렵게 함.
    - `LangChainOpenAIHandler.chat_completion` 메소드 시그니처에 `BaseAiHandler`의 추상 메소드에 정의된 `img_path` 파라미터 누락으로 LSP 위반.
    - (참고) `async def chat_completion` 내 동기적 `self.chat()` 호출로 인한 이벤트 루프 블로킹 문제 인지 (별도 이슈로 다룰 예정).  

  - **주요 작업**
    - `LangChainOpenAIHandler.__init__`에서 `langchain` 미설치 시 명시적 `ImportError` 발생시켜 오류 리포팅 명확화.
    - `LangChainOpenAIHandler.chat_completion` 시그니처에 `img_path: Optional[str] = None` 추가 및 미사용 시 경고 로깅.
    - 모든 `BaseAiHandler` 구현체의 `chat_completion` 시그니처에 `img_path` 파라미터를 추가하여 인터페이스 일관성 확보.

  - **기대 효과**
    - `langchain` 의존성 누락 시 사용자에게 명확한 오류 메시지 제공.
    - 모든 AI 핸들러에서 `chat_completion` 메소드 시그니처 일관성을 확보하여 LSP 준수 및 타입 안정성 향상.
    - 향후 `LangChainOpenAIHandler`의 완전한 비동기 처리를 위한 기반 마련.  

#

- `pr_processing.py`의 `generate_full_patch()` 함수 기능 분해
[Issue #1788](https://github.com/qodo-ai/pr-agent/issues/1788)

  - **내용**
    - `generate_full_patch()` 함수가 파일 스킵 여부 확인, 토큰 제한 검증, 패치 포맷팅, 파일 목록 관리, 로깅 등  
    다중 책임을 가져 코드 복잡성 및 유지보수 어려움 발생.
    - 중첩된 조건문으로 가독성이 낮고, 큰 함수 크기로 인해 테스트 용이성 저하 및 코드 재사용이 제한됨.

  - **주요 작업**
    - `generate_full_patch()` 함수의 각 논리적 단위를 명확한 역할을 가진 여러 헬퍼 함수로 분리  
    (예: `should_skip_file()`, `is_token_limit_exceeded()`, `is_patch_too_large()`, `format_patch()`).
    - 메인 `generate_full_patch()` 함수는 분리된 헬퍼 함수들을 호출하는 방식으로 재구성하여 로직 흐름을 단순화.

  - **기대 효과**
    - 각 헬퍼 함수의 목적이 명확해져 코드 가독성 향상.
    - 특정 로직 수정 시 해당 헬퍼 함수만 변경하면 되어 유지보수 용이성 증대.
    - 각 헬퍼 함수를 독립적으로 단위 테스트할 수 있어 테스트 커버리지 확보 및 테스트 효율성 향상.
    - 헬퍼 함수의 재사용 가능성 증대 및 전체적인 디버깅 용이성 개선.

## 참여자

- 3조, 조원 모두 참여

## 2주 플랜

### 5주차

- 개인별 [qodo-ai/pr-agent](https://github.com/qodo-ai/pr-agent/issues)에 issues **1개** 등록
  - **5/18(일) 정기 미팅 이전**까지
- 등록한 이슈에 대하여 조원분들 모두 **해당 내용 파악**

### 6주차

- 5/19(월) ~ 5/24(토) 비정기적으로 **조원간 개별 미팅**
  - 이슈 해결 시, 도움이 필요한 영역은 서로간의 지식 공유
- 이슈 해결 과정 **전체 공개**로 **코드 리뷰** 요청
  - [ossca-2025/pr-agent-mentoring](https://github.com/ossca-2025/pr-agent-mentoring/pulls)
- 3조 조원은 (답변의 내용/형식 상관없이) **개인 의견 필수** 제출
- 수정/보완 & **멘토님에게 컨펌** 요청
- qodo-ai/pr-agent로 **최종 PR** 요청
