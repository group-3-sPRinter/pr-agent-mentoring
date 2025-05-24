# 6주차 5조 과제

## **과제 목표**

PR-Agent의 review 에 코드 내 TODO 주석을 감지하고 이를 Description에 명시해주는 기능을 구현

---

## 개발자 피드백

https://github.com/qodo-ai/pr-agent/issues/1763

`review` 툴에 섹션을 추가하고,  `TODO` 를 포함하는 코드를 강조하는 방법 제안

![image](https://github.com/user-attachments/assets/1782a37a-2654-488d-95bf-79402a58d8aa)
![image](https://github.com/user-attachments/assets/aafd7b10-5fda-4209-a795-932b938d85d6)


---

## 구현 방법
https://github.com/PullPullers/pr-agent-qodo/pull/1

1. PR에 포함된 변경 파일 (diff) 내에서 `TODO` 또는 `FIXME`로 시작하는 주석을 prompt 로 추출
    - [`pr_agent/settings/pr_reviewer_prompts.toml`](https://github.com/PullPullers/pr-agent-qodo/pull/1/files#diff-2e1ca45d2d8635b5f9cde801d9d210f6516e7f15d45dd9b0be134ac91e7a2e63)
        - 해당 주석이 발견될 경우 다음 정보 수집
            - `relevant_file` : 주석이 위치한 **파일 경로**
            - `line_number` : 해당 주석의 **라인 번호**
            - `content` 주석 **내용**
        - todo_sections 에 리스트업할 수 있게 리스트 포맷으로 변환
            
            ```toml
            {%- if require_todo_scan %}
                todo_sections: Union[List[TodoSection], str] = Field(description="A list of TODO comments found in the code. Return 'No' (as a string) if there are no TODO comments.")
            {%- endif %}
            ```
            
2. 수집한 정보를 PR Description 의 TODO section 에 추가
    - [`pr_agent/algo/utils.py`](https://github.com/PullPullers/pr-agent-qodo/pull/1/files#diff-6b9df72d53c6f0d89fb142c210238a276c0782305e0024d16fbfcaf72c2e2b53)
        - GFM(GitHub Flavored Markdown) 일 때 / 아닐 때 각각 TODO section 생성
        
        ```python
        elif 'todo sections' in key_nice.lower():
            def format_todo_item(todo_item):
                relevant_file = todo_item.get('relevant_file', '').strip()
                line_number = todo_item.get('line_number', '')
                content = todo_item.get('content', '')
                reference_link = None
                try:
                    if git_provider and relevant_file and line_number:
                        reference_link = git_provider.get_line_link(relevant_file, line_number, line_number)
                except Exception as e:
                    print(f"Error generating link: {e}")
                    return f"{relevant_file} [{line_number}]: {content}"
        
                file_line = f"{relevant_file} [{line_number}]"
                if reference_link:
                    if gfm_supported:
                        file_line = f"<a href='{reference_link}'>{file_line}</a>"
                    else:
                        file_line = f"[{file_line}]({reference_link})"
                return f"{file_line}: {content}" if content.strip() else file_line
        
            if gfm_supported:
                markdown_text += f"<tr><td>"
                if is_value_no(value):
                    markdown_text += f"{emoji}&nbsp;<strong>No ToDo sections</strong>"
                else:
                    markdown_text += f"{emoji}&nbsp;<strong>ToDo sections</strong><br><br>\n\n"
                    if isinstance(value, list):
                        markdown_text += "<ul>\n"
                        for todo_item in value:
                            markdown_text += f"<li>{format_todo_item(todo_item)}</li>\n"
                        markdown_text += "</ul>\n"
                    else:
                        markdown_text += f"<p>{format_todo_item(value)}</p>\n"
                markdown_text += f"</td></tr>\n"
            else:
                if is_value_no(value):
                    markdown_text += f"### {emoji} No ToDo sections\n\n"
                else:
                    markdown_text += f"### {emoji} ToDo sections\n\n"
                    if isinstance(value, list):
                        for todo_item in value:
                            markdown_text += f"- {format_todo_item(todo_item)}\n"
                    else:
                        markdown_text += f"- {format_todo_item(value)}\n"
        ```
        
3. 설정 파일(`configuration.toml`)에서 사용자가 활성화 여부 지정 (default : true)
    
    ```
    require_todo_scan=true
    ```
    
4. 간단한 사용 가이드 및 설정 방법을 문서에 추가
    
    ```markdown
    <td><b>require_todo_scan</b></td>
    <td>If set to true, the tool will add a section that lists TODO comments found in the PR code changes. Default is true.
    </td>
    ```
    

---

## 결과 비교

### Before

![image](https://github.com/user-attachments/assets/814c8519-a43f-424a-a3e6-3affd0c33ebc)

![image](https://github.com/user-attachments/assets/89af02b1-f10b-43bb-bcfd-b5776b5227a8)


### After

- `TODO` 섹션에 `파일[줄 번호] : 주석` 형태로 TODO 만 추출하여 보여 줌.

![image](https://github.com/user-attachments/assets/7b490352-b0b6-42bf-94c4-4cd0f56ab731)


- `TODO` 주석이 없는 경우 다음과 같이 No TODO sections

![image](https://github.com/user-attachments/assets/12a07c81-68f7-479f-a036-a2ae36b75022)


---

## 진행상황

**주요 마일스톤**

- [x]  기능 제안 및 이슈 등록
    - 관련 링크: [qodo-ai/pr-agent#1763](https://github.com/qodo-ai/pr-agent/issues/1763)
- [x]  코드 구조 분석 및 주요 로직 리딩
- [x]  TODO 파싱 기능 프로토타입 구현
- [x]  PR Description 생성 로직 확장
- [x]  설정 옵션 추가 및 연동
- [x]  테스트 및 리팩토링
- [x]  기능 문서화
- [ ]  멘토님 피드백
- [ ]  최종 PR 작성

**~~1주차 : 기획 및 분석~~**

- ~~기간 : 05.18 (일) 마감~~
- ~~주요 목표~~
    - ~~PR-Agent 코드 베이스에 대한 이해 및 구조 분석~~
    - ~~팀 레포지토리(PullPuller/pr-agent)에서 각자 브랜치를 생성하여 **기능별 프로토타입 구현**~~
    - ~~구현한 코드에 대해 PR을 생성하고, 정기회의가 끝난 뒤 코드 리뷰 및 의견 취합을 위한 회의 진행~~

**2주차 : 통합 및 테스트**

- 기간 : 05.25(일) 마감
- 주요 목표
    - 1주차의 구현 결과를 팀 회의를 통해 **기능 취합 및 코드 통합**
    - 취합한 코드 및 기능 구현에 대해서 멘토님께 리뷰 요청 → 피드백 반영
    - pr-agent 오픈소스 내 최종 PR 게시 (사용자 가이드 포함 문서 작성)
