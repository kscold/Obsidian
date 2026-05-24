- Reflection = [[AI Agent|에이전트]]가 자신이 만든 출력이나 행동을 **스스로 비평하고 개선**하는 절차. "한 번 더 검토" 단계를 LLM에 위임하는 것.
- 같은 LLM이 검토자가 될 수도 있고, 다른(보통 더 강한) LLM이 검토자가 될 수도 있다.

## 가장 단순한 구조

```
Generate → Critique → Revise → (선택) 반복
```

```python
def reflect_once(task: str) -> str:
    draft = llm.invoke(f"작업: {task}\n초안을 작성해라")
    critique = llm.invoke(f"초안: {draft}\n이 답변의 결함을 비판해라")
    revised = llm.invoke(f"초안: {draft}\n비평: {critique}\n반영해 개선해라")
    return revised
```

## LangGraph 반복 패턴

```python
def generator(state):
    return {"draft": llm.invoke(state["task"])}

def critic(state):
    return {"critique": llm.invoke(f"비평: {state['draft']}")}

def is_good(state):
    return END if "OK" in state["critique"] else "generator"

graph.add_node("generator", generator)
graph.add_node("critic", critic)
graph.add_edge("generator", "critic")
graph.add_conditional_edges("critic", is_good, {"generator": "generator", END: END})
```

## 효과적인 시나리오

- **코드 생성** — 코드가 컴파일·테스트 통과할 때까지 반복.
- **글쓰기** — 톤, 길이, 일관성 검토.
- **수학·논리** — 추론 단계 검증.

## 효과가 거의 없는 시나리오

- 사실관계가 외부 데이터에 있는 경우 → 검증 LLM도 같은 환각을 공유. 이땐 [[Tool Calling|도구]]로 사실 확인이 필요.
- 도메인 지식이 부족한 영역 → 검토가 형식적인 칭찬으로 끝남.

## Reflection vs [[Reflexion]]

- Reflection은 **단일 작업** 안에서의 자기 검토.
- [[Reflexion]]은 **여러 시도에 걸친** 자기 피드백을 메모리로 누적 — 강화학습 신호처럼 다음 시도를 개선.

## 관련

- [[Plan-and-Execute]]의 replanner는 일종의 reflection.
- [[Evaluator-Optimizer]] — 워크플로우 버전.
