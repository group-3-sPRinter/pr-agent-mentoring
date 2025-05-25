# 6주차 조별 과제 (3조 - sPRinter)

## GitHub Organization

- [3조 - sPRinter](https://github.com/group-3-sPRinter)

## 3조 Notion

3조 sPRinter의 **모든 활동 기록**
(출석부, 회의록, GitHub, 발표 내용, 개인별 학습 내용 모음)은
아래 Notion 페이지에서 확인하실 수 있습니다.

- [3조 Notion - sPRinter](https://sunghothegamebird.notion.site/PR-Agnet-3-sPRinter-1d92ec95ce928080a5abeb2e03b5247d?pvs=4)

## 참여 인원

- 강경만 [@Kkan9ma](https://github.com/Kkan9ma)
- 박성호 [@HOPARKSUNG](https://github.com/HOPARKSUNG)
- 서강문 [@KangmoonSeo](https://github.com/KangmoonSeo)
- 손찬우 [@gagip](https://github.com/gagip)
- 이승민 [@Akileox](https://github.com/Akileox)
- 장현상 [@TaskerJang](https://github.com/TaskerJang)

---

## 주차별 회의록

- [회의록](https://sunghothegamebird.notion.site/1fc2ec95ce9280869b40f1665e6095d2?pvs=4)

---

## 3조 조별 발표

- [2주 차 조별 과제 - 강경만 발표](https://sunghothegamebird.notion.site/2-1fc2ec95ce92807da692c31c02439511?pvs=4)
- [3주 차 조별 과제 - 서강문 발표](https://sunghothegamebird.notion.site/3-1fc2ec95ce9280bbb18afc3207061c2d?pvs=4)
- [4주 차 조별 과제 - 박성호 발표](https://sunghothegamebird.notion.site/4-1fc2ec95ce928054b520d91438cbaff8?pvs=4)
- [5주 차 조별 과제 - 손찬우 발표](https://sunghothegamebird.notion.site/5-1fc2ec95ce9280b4a8a1dc00bc843295?pvs=4)

---

## 3조 개인별 활동 정리

- [강경만](https://sunghothegamebird.notion.site/1fc2ec95ce9280f6a5c9f9f4a553eb54?pvs=4)
- [박성호](https://sunghothegamebird.notion.site/1fc2ec95ce928076b6e9dd1322d9c6fd?pvs=4)
- [서강문](https://sunghothegamebird.notion.site/1fc2ec95ce92807eb779c96eb95e168b?pvs=4)
- [손찬우](https://sunghothegamebird.notion.site/1fc2ec95ce92800db477e37d8992bd26?pvs=4)
- [이승민](https://sunghothegamebird.notion.site/1fc2ec95ce92808aae1dc6e8134e93fb?pvs=4)
- [장현상](https://sunghothegamebird.notion.site/1fc2ec95ce92801f8f4add00e7055788?pvs=4) + [개인 블로그](https://velog.io/@tasker_dev/posts)

---

## 3조 (sPRinter) 최종 기여 활동: `algo` 디렉터리 리팩토링

---

### 활동 1: `Token Handler` 개선

- **담당자**: **강경만**
- **코드 리뷰**: **3조 전원**
- **목표**: `pr_agent/algo/token_handler.py` 내의 `count_tokens` 메서드 구조를 개선하여 확장성과 유지보수성을 높입니다.

#### 문제 제기: Issue #1782

- **Issue**: [Proposal: Refactor count_tokens method structure in token_handler.py for better extensibility #1782](https://github.com/qodo-ai/pr-agent/issues/1782)

**내용 요약**

- **현재 문제점**
    - `count_tokens` 메서드는 `force_accurate` 플래그에 따른 분기, 모델 타입 확인, 모델별 토큰 계산 함수 라우팅 등 여러 역할을 수행합니다.
    - 특히, Claude 모델과 비-Claude 모델을 구분하는 방식이 직접적인 조건문으로 되어 있어, 새로운 모델 추가 시 여러 곳의 로직 수정이 필요할 수 있습니다.
    - Maintainer도 현재 구조를 **'hacky'** 하다고 평가했습니다.
- **개선 동기**
    - 관심사를 분리하여 코드베이스의 확장성을 높이고,
    - 새로운 모델 지원 시 변경 사항을 최소화하여 유지보수성을 향상시키고자 합니다.
- **제안된 개선안**
    - `count_tokens` 메서드는 핵심 흐름에 집중하고, 모델별 토큰 계산 로직을 **별도의 핸들러나 레지스트리로 위임**합니다.
    - 이를 통해 모듈화된 구조를 만들고, 새 모델 추가 시 해당 모델의 구성 요소만 수정하도록 합니다.

#### 팀 내부 코드 리뷰: Improve/token handler #1

- **내부 PR**: [group-3-sPRinter/pr-agent#1](https://github.com/group-3-sPRinter/pr-agent/pull/1)

**주요 변경 사항**

- **`ModelTypeValidator` 클래스 추가**: 모델 타입 검증 로직을 분리하여 중앙 집중화.
- **메서드 분리**: `_get_token_count_by_model_type`, `_apply_estimation_factor` 등 모델별/기능별 로직 분리.
- **상수화**: `CLAUDE_MODEL`, `CLAUDE_MAX_CONTENT_SIZE` 등 매직 넘버를 상수로 정의.
- **코드 개선**: `get_settings()` 호출 최소화 (초기 제안, 후반부 리뷰 반영하여 일부 복구), Private 메서드 네이밍(`_`), 타입 힌트 추가.

**주요 리뷰 내용 & 반영**

- **긍정적 피드백**
    - 코드 구조화 및 확장성 향상 (장현상, 이승민).
    - 가독성 및 코드 품질 개선 (장현상, 이승민).
    - 로직 분리 및 상수 사용의 명확성 (이승민, 박성호).
- **개선 제안 & 토론**
    - **오류 처리 추가**: 인코딩 단계 등 (장현상) - 추후 고려.
    - **성능 개선**: 정규식 패턴 컴파일 (장현상) - 추후 고려.
    - **메서드 명명**: `is_openai_model` vs `is_gpt_model` (서강문) - 회사명(OpenAI, Anthropic)으로 통일하는 방향으로 수정.
    - **분기 흐름**: `if not force_accurate:` 유지 (서강문) - 가독성 vs 유지보수성 토론 후, 기존 스타일 유지로 수정.
    - **`get_settings()` 사용**: 기존 프로젝트 스타일 유지 (서강문) - `self.settings` 사용에서 기존 방식대로 `get_settings()` 직접 호출로 수정.
    - **Private 메서드**: `_` 접두사 사용 제안 (손찬우) - 반영하여 수정.
    - **Lazy Import**: 기존 코드의 의도 파악 및 현재 수정의 적절성 논의 (손찬우).
- **리뷰 반영**
    - 피드백을 바탕으로 메서드명 변경, `force_accurate` 로직 복구, `get_settings()` 사용 복구, Private 메서드 컨벤션 적용 등 여러 차례의 커밋을 통해 코드를 개선했습니다.

#### 최종 PR 제출: PR #1805

- **PR**: [Refactor count_tokens method structure in token_handler.py for better extensibility #1805](https://github.com/qodo-ai/pr-agent/pull/1805)

**내용 요약**

- **Issue #1782** 를 해결하기 위한 PR입니다.
- **주요 변경 사항**
    - `ModelTypeValidator` 클래스를 도입하여 모델 타입 검사 로직을 분리.
    - `_get_token_count_by_model_type` 메서드를 통해 모델별 토큰 계산 로직을 위임.
    - `_apply_estimation_factor` 메서드로 부정확한 토큰 계산 시 추정치 적용 로직 분리.
    - Claude 모델명 및 최대 콘텐츠 크기를 상수로 정의.
    - 타입 힌트 추가 및 내부 메서드(`_`) 명명 규칙 적용으로 가독성 및 유지보수성 향상.
- `pr-agent` 봇을 통해 자동 코드 리뷰 및 개선 제안을 받았습니다.

**Maintainer & Bot 피드백**

- **PR Agent Bot**
    - **티켓 준수**: Issue #1782의 요구사항을 **완전히 준수**한다고 평가.
    - **검토 영역 제안**:
        - API 키 처리: `get_settings` 호출을 `ModelTypeValidator`로 통합 고려.
        - 오류 처리: `_calc_claude_tokens`의 오류 시 `max_tokens` 반환 방식에 대한 재고려.
    - **코드 제안**: 정규식 패턴 수정, 정적 메서드 접근 방식 수정 등.
- **Maintainer (mrT23)**
    - `ModelTypeValidator`의 **정적 메서드 호출 방식**을 인스턴스 호출 대신 클래스 직접 호출로 변경 제안 (`self.model_validator.is_openai_model` -> `ModelTypeValidator.is_openai_model`).
    - > "if ModelTypeValidator has static methods, it makes more sense, and its cleaner, to just call it directly. otherwise, the PR looks good. fix this and we can merge"
    - 해당 피드백을 반영하여 수정 후 **Merge 대기 중**입니다.

#### 결론 및 성과

- `token_handler.py`의 `count_tokens` 메서드의 **확장성과 유지보수성을 성공적으로 개선**했습니다.
- 팀 내부의 **활발한 코드 리뷰**를 통해 다양한 관점에서 코드를 검토하고 개선 방향을 논의하며 **협업 역량을 강화**했습니다.
- `pr-agent` 봇과 **Maintainer로부터 긍정적인 피드백**을 받고, 최종 Merge를 위한 단계를 진행하고 있습니다.
- 이번 활동을 통해 오픈소스 프로젝트 기여 프로세스를 직접 경험하고, 코드 품질 향상에 기여하는 값진 경험을 쌓았습니다.

---

### 활동 2: `AI Handler` 예외 처리 및 재시도 로직 개선

- **담당자**: **서강문**
- **코드 리뷰**: **3조 전원**
- **목표**: `pr_agent/algo/ai_handlers/*.py` 내의 예외 처리 순서 및 `@retry` 데코레이터 동작을 수정하여, `RateLimitError` 발생 시 불필요한 재시도를 방지하고 의도된 대로 즉시 예외를 발생시키도록 개선합니다.

#### 문제 제기 1: Issue #1802 - `LiteLLMAIHandler` 예외 처리 순서 오류

- **Issue**: [Incorrect exception order in LiteLLMAIHandler prevents proper @retry behavior #1802](https://github.com/qodo-ai/pr-agent/issues/1802)

**내용 요약**

- **현재 문제점**
    - `LiteLLMAIHandler.chat_completion()` 메서드에서 `openai.RateLimitError`가 부모 클래스인 `openai.APIError`보다 **나중에** 처리되도록 `except` 블록이 구성되어 있습니다.
    - `RateLimitError`는 `APIError`를 상속받기 때문에, `RateLimitError`가 발생해도 `APIError` 블록에서 먼저 잡혀버립니다.
- **영향**:
    - `@retry` 데코레이터는 `RateLimitError` 발생 시 재시도하지 않도록 설정되어 있지만, `APIError`로 인식되어 **불필요한 재시도**가 발생합니다.
    - `RateLimitError`가 `error` 레벨이 아닌 `warning` 레벨로 로깅됩니다.

#### PR 제출 1 & Merge: PR #1803 - 예외 처리 순서 수정

- **PR**: [fix: reorder exception handling in LiteLLMAIHandler.chat_completion() #1803](https://github.com/qodo-ai/pr-agent/pull/1803)

**내용 요약**

- **Issue #1802** 를 해결하기 위해 `except` 블록의 순서를 변경했습니다.
- `RateLimitError`를 `APIError`보다 **먼저** 처리하도록 수정하여, `RateLimitError`가 발생했을 때 올바른 블록에서 처리되도록 했습니다.
- 이를 통해 재시도 로직이 의도대로 동작하고 올바른 로깅 레벨이 사용되도록 보장합니다.
- Maintainer(mrT23)가 **"a good catch"** 라며 긍정적인 피드백과 함께 **신속하게 Merge** 해주었습니다.

#### 문제 제기 2: Issue #1807 - `@retry` 데코레이터의 `RateLimitError` 처리 오류

- **Issue**: [RateLimitError incorrectly retried due to APIError inheritance in @retry decorator #1807](https://github.com/qodo-ai/pr-agent/issues/1807)

**내용 요약**

- **현재 문제점**
    - PR #1803에서 `except` 블록 순서는 수정했지만, `@retry` 데코레이터 **자체**가 `RateLimitError`를 잘못 처리하는 근본적인 문제가 남아있었습니다.
    - `@retry(retry=retry_if_exception_type(openai.APIError))` 설정 때문에, `APIError`를 상속하는 `RateLimitError`도 재시도 대상에 포함되었습니다.
- **영향**
    - 주석(`# No retry on RateLimitError`)과 달리, `RateLimitError` 발생 시 **여전히 불필요한 재시도**가 5번 수행되었습니다.

#### 팀 내부 코드 리뷰: fix: exclude RetryLimitError from retry logic #2

- **내부 PR**: [group-3-sPRinter/pr-agent#2](https://github.com/group-3-sPRinter/pr-agent/pull/2)

**주요 변경 사항 & 리뷰 내용**

- **`tenacity` 라이브러리 일관 적용**: 모든 AI 핸들러에서 `tenacity`의 `retry`를 사용하도록 통일했습니다.
- **`@retry` 조건 수정**: `retry_if_not_exception_type`을 사용하여 `APIError`는 포함하되, `RateLimitError`는 **명시적으로 제외**하도록 수정했습니다.
- **예외 처리 패턴 표준화**: 모든 AI 핸들러에 `RateLimitError(error) -> APIError(warn) -> Exception(warn)` 구조를 적용했습니다.
- **테스트 결과 공유**: AS-IS와 TO-BE 상황에서의 재시도 동작을 명확히 비교하여 변경 사항의 효과를 입증했습니다.
- **팀원 피드백**:
    - `tenacity` 적용 및 로직 개선에 대한 긍정적 평가 (강경만, 이승민).
    - 예외 처리 순서 및 `Timeout` 처리 관련 질문 및 논의 (강경만).
    - 중복 `import` 제거 제안 (장현상).
    - 전반적인 코드 완성도 및 테스트 방식에 대한 긍정적 평가 (이승민, 박성호).

#### 최종 PR 제출 2 & Merge: PR #1808 - `@retry` 로직 수정

- **PR**: [fix: exclude RateLimitError from @retry in AIHandler.chat_completion() #1808](https://github.com/qodo-ai/pr-agent/pull/1808)

**내용 요약**

- **Issue #1807** 을 해결하기 위한 PR입니다.
- 모든 AI 핸들러(`OpenAI`, `LiteLLM`, `LangChain`)에 걸쳐 `tenacity`를 사용하여 `@retry` 데코레이터가 `RateLimitError`를 **명시적으로 제외**하도록 수정했습니다.
- 예외 처리 및 로깅 패턴을 일관성 있게 표준화했습니다.
- Maintainer(mrT23)가 **"thanks"** 와 함께 **신속하게 Merge** 해주었습니다.

#### 결론 및 성과

- 두 차례의 Issue 제기 및 PR 제출을 통해 `pr-agent`의 AI 핸들러에서 **`RateLimitError` 처리와 관련된 버그를 성공적으로 수정**했습니다.
- 예외 처리 상속 관계를 명확히 이해하고, `tenacity` 라이브러리를 활용하여 **더욱 정교하고 안정적인 재시도 로직을 구현**했습니다.
- 팀 내부 리뷰와 테스트 결과 공유를 통해 **변경 사항의 타당성을 검증하고 코드 품질을 향상**시켰습니다.
- Maintainer와의 빠른 소통과 피드백 반영을 통해 **오픈소스 프로젝트에 성공적으로 기여**하고 Merge를 이끌어냈습니다.

---

### 활동 3: `utils.py`의 `clip_tokens` 함수 테스트 및 문서화 강화

- **담당자**: **장현상**
- **코드 리뷰**: **3조 전원**
- **목표**: `pr_agent/algo/utils.py` 내의 `clip_tokens` 유틸리티 함수의 단위 테스트를 추가하고, 문서(docstring)를 개선하여 코드의 신뢰성과 개발자 경험을 향상시킵니다.

#### 문제 제기: Issue #1793 - `clip_tokens` 테스트 및 문서화 필요성

- **Issue**: [Add Unit Tests and Improve Documentation for utils.py clip_tokens Function #1793](https://github.com/qodo-ai/pr-agent/issues/1793)

**내용 요약**

- **현재 상태**
    - `clip_tokens` 함수는 텍스트를 지정된 토큰 수로 제한하는 중요한 유틸리티이지만, 포괄적인 테스트가 부족합니다.
    - Docstring이 일부 매개변수(`add_three_dots`, `num_input_tokens`, `delete_last_line`)에 대한 설명이 누락되어 있고 사용 예제가 없습니다.
    - 예외 처리가 포괄적인 `except Exception`으로 되어 있어 특정 오류를 파악하기 어렵습니다.
- **개선 필요성**
    - **테스트 커버리지**: 다양한 입력과 엣지 케이스에 대한 동작을 검증하고, 향후 회귀(regression)를 방지하기 위해 단위 테스트가 필요합니다.
    - **문서화**: 개발자가 함수의 매개변수와 동작을 명확히 이해하고 올바르게 사용할 수 있도록 docstring을 개선하고 예제를 추가해야 합니다.
    - **예외 처리**: (제안되었으나 이번 PR 범위에서는 제외됨) 특정 예외(예: `ZeroDivisionError`)를 처리하여 견고성을 높일 필요가 있습니다.
- **Maintainer 피드백**: Maintainer(mrT23)가 **"adding unit tests and updating the docstring is welcome"** 이라며 긍정적인 반응을 보였습니다.

#### 팀 내부 코드 리뷰: Add Unit Tests and Improve Documentation ... #3

- **내부 PR**: [group-3-sPRinter/pr-agent#3](https://github.com/group-3-sPRinter/pr-agent/pull/3)

**주요 변경 사항 & 리뷰 내용**

- **단위 테스트 추가**: `tests/unittest/test_clip_tokens.py`에 **총 21개의 포괄적인 단위 테스트 케이스**를 추가했습니다.
    - (빈 문자열, 음수 토큰, 유니코드, Mock 테스트 등 다양한 엣지 케이스 포함)
- **Docstring 개선**: `clip_tokens` 함수의 docstring을 모든 매개변수 설명과 4개의 사용 예제를 포함하여 **완전히 개선**했습니다.
- **하위 호환성 유지**: 기존 함수의 로직은 변경하지 않아 안정성을 보장했습니다.
- **팀원 피드백**
    - 매우 꼼꼼한 테스트 케이스에 대한 긍정적 평가 (서강문).
    - 커밋 메시지를 영어로 작성할 것을 제안 (서강문).
    - 테스트 케이스 작성 과정(생각의 흐름)에 대한 질문 및 공유 (박성호) - 핵심 기능 -> 엣지 케이스 -> 매개변수 조합 -> 예외 순으로, Issue 요구사항을 기반으로 점진적으로 추가했음을 공유.

#### 최종 PR 제출: PR #1816 - `clip_tokens` 테스트 및 문서화

- **PR**: [Add Unit Tests and Improve Documentation for utils.py clip_tokens Function #1816](https://github.com/qodo-ai/pr-agent/pull/1816)

**내용 요약**

- **Issue #1793** 을 해결하기 위한 PR입니다.
- **21개의 단위 테스트 추가**와 **Docstring 개선**을 통해 `clip_tokens` 함수의 신뢰성과 사용성을 크게 향상시켰습니다.
- `pr-agent` 봇은 Issue 요구사항 중 테스트와 문서화는 충족했지만, 예외 처리 개선은 포함되지 않았다고 분석했습니다 (이는 의도된 범위).
- 봇은 테스트 코드에서 `== None` 대신 `is None`을 사용할 것을 제안했습니다.

**Maintainer & Bot 피드백**

- **PR Agent Bot**
    - **티켓 준수**: 부분적 준수 (테스트/문서화 O, 예외 처리 X).
    - **코드 제안**: `== None` 대신 `is None` 사용 제안.
- **Maintainer (mrT23)**
    - 봇의 제안에 동의하며 **`is None` 사용을 권장**했습니다.
    - > "is is a better practice"
    - 해당 피드백을 반영하여 **코드를 수정하고 커밋**했습니다 (`fix: use identity check for None comparison...`).
    - 현재 **Merge 완료!**

#### 결론 및 성과

- `clip_tokens` 유틸리티 함수에 대한 **21개의 포괄적인 단위 테스트를 성공적으로 추가**하여 코드의 안정성과 신뢰도를 크게 높였습니다.
- **Docstring을 상세하게 개선**하여 다른 개발자들이 함수를 더 쉽고 정확하게 이해하고 사용할 수 있도록 기여했습니다.
- 팀 내부 리뷰를 통해 **테스트 작성 노하우를 공유**하고, 외부(봇, Maintainer) 피드백을 신속하게 반영하여 **오픈소스 협업 프로세스를 성공적으로 수행**했습니다.
- 이번 기여를 통해 `pr-agent` 프로젝트의 **테스트 커버리지를 향상**시키고 **코드 품질 유지에 기여**했습니다.

---

### 활동 4: `LangChain Handler` 개선

- **담당자**: **이승민**
- **코드 리뷰**: **3조 전원** (진행 중)
- **목표**: `pr_agent/algo/ai_handlers/langchain_ai_handler.py`의 안정성, 인터페이스 호환성 및 비동기 무결성을 개선합니다.

#### 문제 제기: Issue #1784

- **Issue**: [Refactor LangChainOpenAIHandler for Robustness and Interface Compliance #1784](https://github.com/qodo-ai/pr-agent/issues/1784)

**내용 요약**

- **현재 문제점**
    - **Optional Dependency 처리**: `langchain` 라이브러리가 설치되지 않았을 때 `ImportError`가 숨겨져, 런타임 시 `NameError`가 발생하여 진단이 어렵습니다.
    - **Interface 불일치**: `LangChainOpenAIHandler.chat_completion` 메서드에 `BaseAiHandler` 인터페이스의 `img_path` 매개변수가 누락되어 LSP(리스코프 치환 원칙)를 위반합니다.
    - **동기/비동기 문제**: `async` 메서드 내에서 동기(blocking) `invoke`를 호출하여 `asyncio` 이벤트 루프를 차단합니다.
- **제안된 개선안**
    - **Dependency 처리**: `_LANGCHAIN_INSTALLED` 플래그를 사용하여 `__init__` 시 명시적인 `ImportError`를 발생시킵니다.
    - **Interface 수정**: 모든 AI 핸들러의 `chat_completion`에 `img_path`를 추가하고, 미지원 시 경고를 로깅합니다.
    - **Async 수정**: `ainvoke`를 사용하여 비동기 호출을 수행하고, LLM 타입에 따라 조건부 매개변수를 전달합니다.

#### 팀 내부 코드 리뷰: Refactor: Enhance AI Handler ... #5

- **내부 PR**: [group-3-sPRinter/pr-agent#5](https://github.com/group-3-sPRinter/pr-agent/pull/5)
- **상태**: **내부 코드 리뷰 진행 중**

**주요 변경 사항**

- **`ImportError` 처리 개선**: `_LANGCHAIN_INSTALLED` 플래그를 도입하여 `langchain` 미설치 시 명확한 오류 메시지를 제공합니다.
- **`img_path` 인터페이스 일치**: 모든 AI 핸들러에 `img_path` 매개변수를 추가하고, 미지원 시 경고를 로깅합니다.
- **비동기 처리 강화**
    - `_create_chat_async` 메서드를 도입했습니다.
    - `await llm.ainvoke()`를 사용하여 비동기 LLM 호출을 수행합니다.
    - LLM 타입(OpenAI 계열 vs 기타)에 따라 `ainvoke`에 전달되는 매개변수를 조건부로 처리합니다.
    - `ainvoke` 미지원 시 `NotImplementedError`를 발생시켜 라이브러리 업데이트를 유도합니다.
- **단위 테스트 추가**: `test_langchain.py` 파일을 추가하여 `ImportError` 처리, `img_path` 경고, 비동기 성능 등을 검증합니다.

#### 결론 및 성과 (예상)

- `LangChain` 의존성 문제 발생 시 **명확한 오류 메시지**를 통해 사용자 경험을 개선합니다.
- 모든 AI 핸들러 간 **인터페이스 일관성**을 확보하여 유지보수성을 향상시킵니다.
- `LangChainOpenAIHandler`의 비동기 처리를 개선하여 **동시 요청 시 성능을 크게 향상**시킵니다.
- 추가된 단위 테스트를 통해 **코드의 안정성과 신뢰성을 확보**합니다.

---