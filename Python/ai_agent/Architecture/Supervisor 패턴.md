- Supervisor 패턴 = 1명의 **매니저(Supervisor) [[AI Agent|에이전트]]** 가 사용자 요청을 받고, 어떤 워커 에이전트에게 작업을 위임할지 결정하고, 결과를 종합해 사용자에게 돌려주는 [[Multi Agent|멀티 에이전트]] 구조.
- 가장 안정적이고 디버깅이 쉬워 production에서 가장 흔히 채택된다.

## 구조

```
              ┌──> Researcher Agent
사용자 → Supervisor ─┼──> Coder Agent
              └──> Reviewer Agent
```

- Supervisor는 워커들의 능력(`description`)을 system prompt로 알고 있고, 매 턴 다음에 누구를 부를지 결정.

## LangGraph 구현 (의사)

```python
from langgraph.graph import StateGraph, START, END
from langchain_core.messages import HumanMessage

class State(TypedDict):
    messages: Annotated[list, add_messages]
    next: str

def supervisor_node(state):
    decision = llm.with_structured_output(Route).invoke([
        SystemMessage("다음 워커 중 하나를 골라라: researcher / coder / reviewer / FINISH"),
        *state["messages"],
    ])
    return {"next": decision.next}

def researcher_node(state): ...
def coder_node(state): ...
def reviewer_node(state): ...

graph = StateGraph(State)
graph.add_node("supervisor", supervisor_node)
graph.add_node("researcher", researcher_node)
graph.add_node("coder", coder_node)
graph.add_node("reviewer", reviewer_node)

graph.add_edge(START, "supervisor")
graph.add_conditional_edges("supervisor", lambda s: s["next"], {
    "researcher": "researcher",
    "coder": "coder",
    "reviewer": "reviewer",
    "FINISH": END,
})
# 모든 워커는 끝나면 다시 supervisor에게 보고
for w in ["researcher", "coder", "reviewer"]:
    graph.add_edge(w, "supervisor")
```

## 장점

- **흐름이 명확** — 모든 결정이 supervisor를 통과해 추적이 쉽다.
- **권한 격리** — 워커는 자기 역할 범위 안에서만 행동.
- **종료 제어** — supervisor가 `FINISH`를 결정하므로 무한 루프가 잘 안 난다.

## 단점

- Supervisor의 판단 품질이 곧 시스템 품질 (병목).
- 워커가 많아지면 라우팅 정확도 ↓ → [[Hierarchical Agent|계층 구조]]로 쪼개야 함.

## 권장 운영 팁

- Supervisor에 **structured output** 으로 다음 워커를 강제. 자연어 자유 형식은 깨지기 쉽다.
- 워커마다 `description`을 한 줄로 명확히 — supervisor가 보는 메뉴판.
- 최대 turns 한도 설정.

## 관련

- [[Multi Agent]] · [[Swarm 패턴]] · [[Hierarchical Agent]].
- [[LangGraph]] · [[CrewAI]]는 supervisor 패턴 빌트인 지원.
