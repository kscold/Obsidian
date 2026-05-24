- Self-Improving Agent = **에이전트가 사용 중 스스로의 능력·프롬프트·도구를 진화시키는** 패턴들의 총칭.
- 인간이 매번 새 도구·프롬프트를 짜 넣지 않아도, 에이전트가 경험으로부터 자신을 갱신한다. AWS Strands 발표가 이를 3패턴으로 정리했다.

## Pattern 1 — Self-Extending Agent (도구를 스스로 만든다)

- 에이전트가 자신의 `tools/` 디렉토리에 파일을 쓰고, 다음 호출 시 자동 로딩.

```python
# Strands
agent = Agent(
    tools=[shell],
    load_tools_from_directory=True,   # 디렉토리의 파일을 자동 도구화
)
agent("기상청 API를 호출하는 weather 도구를 만들고 사용해줘")
# → ./tools/weather.py 가 생성 → 다음부터 도구로 등록됨
```

- 핵심 메커니즘: shell 같은 메타 도구 + 도구 디렉토리 자동 발견.

## Pattern 2 — Self-Modifying System Prompt

- 에이전트가 자신의 system prompt를 **읽고·수정하고·저장**한다.

```python
@tool
def update_system_prompt(new_prompt: str) -> str:
    Path("prompts/system.md").write_text(new_prompt)
    return "OK"

agent = Agent(system_prompt=load_prompt(), tools=[update_system_prompt, ...])
agent("앞으로는 모든 답을 격식체로 해. 시스템 프롬프트에 반영해줘.")
# → 파일 갱신 → 다음 세션부터 반영
```

- 위험성: 무한 자기 수정으로 정체성이 표류 → 변경 이력 git 추적·승인 게이트 필수.

## Pattern 3 — Agents that Learn (상호작용으로 학습)

- 사용자 피드백·실패 trajectory를 [[Memory|장기 메모리]]에 저장하고 검색으로 재사용.

```python
def remember(text: str, kind: str):
    store.add_texts([text], metadatas=[{"kind": kind}])

def recall(query: str) -> list[str]:
    return [d.text for d in store.similarity_search(query, k=5)]
```

- [[Reflexion]]의 메모리화 — 같은 실수를 두 번 안 하는 것.

## 공통 메커니즘 — Retrieve · Modify · Persist

```
[Retrieve]  현재 상태(프롬프트/도구/메모리) 로딩
    ↓
[Modify]    LLM이 판단해 변경
    ↓
[Persist]   디스크·DB·git에 저장 (다음 세션에 반영)
```

## 안전장치

- **모든 self-modification은 버전 관리** — git diff로 변경 확인.
- **롤백 경로** — 성능이 떨어지면 이전 버전으로 자동 복귀.
- **[[Evaluation|회귀 평가]] 연동** — 변경 후 평가 점수가 떨어지면 차단.
- 위험 도구 자가 생성은 [[Human-in-the-loop|HITL]] 게이트.

## 어디까지가 "자율"인가

- Strands·AutoGen 류는 코드 단위 자가 생성·실행까지 데모.
- 운영 환경에선 **샌드박스 + 권한 격리** ([[MCP(Model Context Protocol)]] roots, container)가 사실상 필수.

## 관련

- [[Reflexion]] · [[Reflection]] — 학습 메커니즘.
- [[Memory]] — 장기 학습 저장소.
- [[System Prompt]] · [[Tool Calling]] — 수정 대상.
- [[Evaluation]] · [[Guardrails]] — 안전 가드.
