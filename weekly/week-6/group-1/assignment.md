# 6주차 1조 과제

## 에그 베네딕트 5-6주차 활동
### 과제 목표

PR Agent의 `/describe` 도구 문서를 개선하고, add_diagram을 opt-in 방식으로 변경하여 기능 구현


### 과제 제출
(1차 기능 구현) https://github.com/qodo-ai/pr-agent/pull/1824

<img width="504" alt="스크린샷 2025-05-25 오후 5 02 16" src="https://github.com/user-attachments/assets/7bddda0b-88cb-41dd-b162-fa70bdda8b75" />


### 주요 변경 사항
- /describe 도구의 문서 구조를 재정렬 : 핵심 기능 설명을 최상단에 배치하여 가독성과 이해도를 높임
- add_diagram과 같은 옵션 기능과 플래그는 본 설명 하단으로 이동
- configuration.toml 파일에서 add_diagram의 기본값을 false로 변경하여, 명시적으로 활성화해야만 동작하도록 수정

### 1차 피드백
(피드백) https://github.com/qodo-ai/pr-agent/pull/1826

#### 주요 피드백
1. 다이어그램 섹션 분리
   - 팀 아이디어: 파일 수준이 아닌 전체 PR 관점에서 다이어그램을 요약하는 방식을 구상
   - 개발자 분 요청: 파일별 변경사항에 대해 시퀀스 다이어그램을 붙이려는 목적으로 보임

2. 프롬프트 조정
   - 다이어그램 조정에 따른 프롬프트 변경 필요

⸻

### 개인별 역할 요약

#### 1. 김지한
- LLM 기반 시퀀스 다이어그램 기능의 정확성 검증 문제 제기
- AST 기반 함수 호출 관계 분석 스크립트 직접 설계 및 구현
- Mermaid 다이어그램 생성을 위한 정확한 호출 흐름 추출
- 내장 함수 제거, 외부 호출은 [external]로 구분하여 가독성과 의미 강화

⸻

#### 2. 김가희
- 시퀀스 다이어그램 검증 방식 실험 (Tree-sitter vs. LLM 기반)
- Tree-sitter는 정확하나 초기 구현 부담 및 합의 미비로 보류
- LLM 기반 호출 흐름 추출로 방향 전환
- call_flow 데이터가 기존 구조와 충돌하여, 타입 호환 및 리팩토링 필요성 식별

⸻

#### 3. 주동욱
- 프롬프트 설정 중 max_context_tokens 값을 조정하며 LLM 인식 범위에 따른 정확도 실험
- 다양한 token 크기(2K, 4K, 8K+)에 따른 응답 품질 및 속도 비교
- 중간 수준(4K~8K)에서 품질과 성능 간 최적 균형임을 확인
- 향후 adaptive prompt tuning 전략 가능성 제안

⸻

#### 4. 한유진
- Mermaid 다이어그램이 지침에 따라 정확히 생성되고 삽입되는지 프롬프트 기반 검증 체계 수립
- 검증 체크리스트 구성:
  - description 내 정확한 삽입 위치
  - Mermaid 문법(```mermaid, participant, ->>) 준수
  - 함수 호출 흐름 요약/필터링 적용 여부
  - 프롬프트 간 요구사항 충돌 없는지 점검
- 세 가지 프롬프트별 형식, 위치, 의미의 정확성 검증 수행

⸻

#### 5. 김예지
- JAVA 및 JavaScript 코드 기반 Mermaid 다이어그램 품질 검증 테스트 수행
- 단순/복잡 구조 다이어그램 모두 테스트하며 LLM 반응 분석
- 비동기 호출 흐름에서 Mermaid 문법 오류 및 호출 방향 인식 오류 다수 확인
- LLM 컨텍스트를 보강해 핵심 로직 중심 표현 유도 전략 제안

⸻

#### 6. 윤선웅
- Python 및 C++ 기반 코드로 Mermaid 다이어그램 생성 품질 검증
- 단순/복잡 구조 다이어그램 기준으로 프롬프트 적용 일관성 및 호출 흐름 정확성 평가
- Python은 비교적 안정적이나, C++에선 participant 구성 오류 및 호출 누락 현상 발생
- 언어별 특성에 맞는 다이어그램 최적화 방향 

⸻

### 추후 목표
- `1차 피드백을 반영한 기능 개선 및 PR 보완 예정` :  다이어그램 삽입 위치, 프롬프트 구조 등에 대한 리뷰 내용을 기반으로 리팩토링 진행
- `1차 PR을 기반으로 개인별 역할에 맞는 기능 확장 진행` : 각자 담당한 실험 및 검증 로직을 실제 기능 구현에 연계


## 개인 활동 정리 – 에그 베네딕트 팀

6주차까지의 개인별 활동 및 기록 문서를 정리한 모음입니다.  
학습 과정, 실습 결과, 구현 기능, 문서 기여 내역 등을 링크 기반으로 확인할 수 있습니다.

---


### 김지한

- **1주차**  
  - [LLM 심층 분석 (ChatGPT 작동 방식)](https://github.com/jihan-chillin/TIL/blob/main/LLM/LLM%20%EC%8B%AC%EC%B8%B5%EB%B6%84%EC%84%9D%20(Chat%20GPT%20%EC%9E%91%EB%8F%99%EB%B0%A9%EC%8B%9D).md)
- **2주차**  
  - [PR Agent + CodeRabbit 비교](https://www.notion.so/PR-Agent-CodeRabbit-1e12c77b05c28065b054d4728e4e4118?pvs=21)
- **3주차**  
  - [환경설정 및 트러블슈팅, CI/CD 배포 파라미터](https://kojub.tistory.com/48)  
  - [Introduction](https://www.notion.so/Introduction-1e62c77b05c2803f9a7fc9e477a76f7b?pvs=21) / [Repo Clone](https://www.notion.so/Repo-Clone-1e62c77b05c280bfbda4c930a38b4646?pvs=21) / [CI/CD 구성](https://www.notion.so/CI-CD-1e72c77b05c28036b448d8e9161e733b?pvs=21)
- **4주차**  
  - [개인 정리 문서](https://www.notion.so/1ee2c77b05c280a1bc4ecf2d4fe03ed3?pvs=21)
- **5주차**  
  - [기능 구현 PR](https://github.com/OSSCA-2025-Egg-Benedict/pr-agent-from-ossca/pull/19)

---

### 김가희
- **1주차 학습** 
  - [LLM 심층 분석](https://velog.io/@aborrencce/LLM-%EC%8B%AC%EC%B8%B5-%EB%B6%84%EC%84%9D)  
- **2주차 학습** 
  - [AI 코드 리뷰 도구 비교](https://velog.io/@aborrencce/AI-%EC%BD%94%EB%93%9C-%EB%A6%AC%EB%B7%B0-%EB%8F%84%EA%B5%AC-%EB%B9%84%EA%B5%90)  
  - [RAG + MCP + A2A 구조 분석](https://velog.io/@aborrencce/RAG-MCP-A2A)  
- **3주차 학습** 
  - [PR-Agent 로컬 환경 설정 1](https://velog.io/@aborrencce/PR-Agent-%EB%A1%9C%EC%BB%AC-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95)  
  - [PR-Agent 로컬 환경 설정 2](https://velog.io/@aborrencce/PR-Agent-%EB%A1%9C%EC%BB%AC-%ED%99%98%EA%B2%BD-%EC%84%A4%EC%A0%95)
- **4주차 학습**
  - [4주차 과제](https://github.com/ossca-2025/pr-agent-mentoring/blob/main/weekly/week-4/group-1/Ideation/llm_reasoning_effort.md)   
---

### 주동욱

- **1주차 학습**  
  - [주동욱 Week 1](https://github.com/OSSCA-2025-Egg-Benedict/pr-agent-mentoring/blob/main/weekly/week-1/group-1/%EC%A3%BC%EB%8F%99%EC%9A%B1.md)
- **2주차 학습** 
  - [주동욱 Week 2](https://www.notion.so/2-1d82c77b05c280b0a9cddc065df5f519?pvs=21)
- **3주차 학습**  
  - [주동욱 Week 3](https://www.notion.so/3-1e52c77b05c2803eaa33d12ad6a852b0?pvs=21)
- **4주차 학습** 
  - [주동욱 Week 4](https://www.notion.so/4-1e92c77b05c280a8b797c9429b89502a?pvs=21)

---

### 한유진

- **1주차 학습**  
  - [한유진 Week 1](https://github.com/OSSCA-2025-Egg-Benedict/pr-agent-mentoring/blob/main/weekly/week-1/group-1/%ED%95%9C%EC%9C%A0%EC%A7%84.md)
- **2주차 학습**  
  - [한유진 Week 2](https://github.com/OSSCA-2025-Egg-Benedict/pr-agent-mentoring/blob/main/weekly/week-2/group-1/%ED%95%9C%EC%9C%A0%EC%A7%84.md)
- **3주차 학습**  
  - [한유진 Week 3](https://www.notion.so/1e72c77b05c2802f8ca3e05232cf2fbd?pvs=21)  
- **4주차 학습**  
  - [한유진 Week 4](https://www.notion.so/1ee2c77b05c280b6ab95eb0fdbb6b50e?pvs=21)

---

### 김예지

- **1주차 학습**  
  - [김예지 Week 1](https://github.com/ossca-2025/pr-agent-mentoring/blob/main/weekly/week-1/group-1/%EA%B9%80%EC%98%88%EC%A7%80.md)  
- **2주차 학습** 
  - [김예지 Week 2](https://github.com/ossca-2025/pr-agent-mentoring/blob/main/weekly/group-1/%EA%B9%80%EC%98%88%EC%A7%80.md)  
- **3주차 학습** 
  - [김예지 Week 3](https://github.com/ossca-2025/pr-agent-mentoring/blob/main/weekly/week-3/group-1/README.md)
- **4주차 학습** 
  - [시퀀스 다이어그램 생성 구현 PR](https://github.com/OSSCA-2025-Egg-Benedict/pr-agent-from-ossca/pull/10)  
- **5주차 학습** 
  - [언어별 다이어그램 검증 노션](https://www.notion.so/1fd2c77b05c280049895e6134d4e6a5b?pvs=21)  
- [PR-Agent 문서 기여 PR](https://github.com/qodo-ai/pr-agent/pull/1760)

---

### 윤선웅

- **1주차 학습**  
  - [윤선웅 Week 1](https://github.com/OSSCA-2025-Egg-Benedict/pr-agent-mentoring/blob/main/weekly/week-1/group-1/%EC%9C%A4%EC%84%A0%EC%9B%85.md)
- **2주차 학습**  
  - [윤선웅 Week 2](https://github.com/OSSCA-2025-Egg-Benedict/pr-agent-mentoring/blob/main/weekly/week-2/group-1/%EC%9C%A4%EC%84%A0%EC%9B%85.md)
- **3주차 학습**  
  - [윤선웅 Week 3](https://github.com/OSSCA-2025-Egg-Benedict/pr-agent-mentoring/blob/main/weekly/week-3/group-1/configurations/features-and-actions.md)
- **4주차 학습**  
  - [윤선웅 Week 4](https://www.notion.so/4-1e92c77b05c280a8b797c9429b89502a?pvs=21)

---
