# 6주차 과제: 구현 완료 및 PR 전송

### 4조 과제 목표

- **기술 문서 기여**: **Installation**과 **Usage Guide** 보강을 통해 신규 사용자의 진입 장벽 완화
- e.g. GitHub Integration 문서: GitHub Actions 워크플로우에서 OpenAI 외 모델 설정 방법 상세 안내 + configuration 파일 변수 설정 방법 구체적 설명
- 코드와 대조해 검증하며 친절하고 자세한 가이드 제공

### 범위

- PR Agent 공식 기술 문서 내에서 기여할만한 사항을 찾아 작업:
  - /pr-agent/docs/docs/installation
  - /pr-agent/docs/docs/usage-guide
    (https://qodo-merge-docs.qodo.ai/)
- 등록한 이슈: [Add detailed configuration examples to GitHub Actions workflows #1780](https://github.com/qodo-ai/pr-agent/issues/1780)

---

### 기여사항 보고

#### 이서현
> 기술 문서에 기여하기 위해 문서를 읽어보는 과정에서, 설정 코드 오류와 업데이트되지 않은 사항을 발견해 수정하는 PR을 올려 머지되었습니다. 또한 주석 및 로그에서 코드 컨벤션을 맞추었습니다.
**기여 결과**
- [PR] 문서 및 로그 메시지의 오타 수정: https://github.com/qodo-ai/pr-agent/pull/1798
- [PR] 리뷰 문서에 설명된 review effort label 텍스트를 최신 버전에 맞게 수정: https://github.com/qodo-ai/pr-agent/pull/1799

#### 정동환
> GitHub Actions 워크플로우 파일들에서 사용하는 Github Actions의 버전을 최신 버전으로 업데이트하는 PR을 올려 머지되었습니다.
**기여 결과**
- [PR] Github Actions 버전 업데이트: https://github.com/qodo-ai/pr-agent/pull/1704

#### 박상민
> GitHub Actions에서 configuration 변경 관련 문서를 기여하려고 했지만, 이미 존재하여 다른 기여 방식을 고민했습니다. 마땅한 것을 발견하지 못했지만, 레포지토리를 탐색하는 과정에서 오타 수정 PR을 올려 머지되었습니다.
**기여 결과**
- [PR] 오타 수정 및 사용하지 않는 import 삭제: https://github.com/qodo-ai/pr-agent/pull/1778
- [PR] 오타 발견 및 maintainer에게 수정 요청: https://github.com/qodo-ai/pr-agent/issues/1725

#### 박영신
> configuration 설정 문서화를 진행중입니다. 아직 올린 기여는 없으나, 기여를 위해 지속할 예정입니다.

#### 김범진
> 문서화 이외의 사항을 탐색했습니다. PR 을 올리지는 않았지만, 좀 더 시간을 들여 Patch 파일을 가져와서 후처리하는 과정에서 테스트 코드를 추가하거나 동적 컨텍스트를 효율화하는 개선사항을 올릴 예정입니다.