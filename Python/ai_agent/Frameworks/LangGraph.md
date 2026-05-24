- LangGraph는 [[AI Agent|에이전트]]를 **노드(Node) + 엣지(Edge)** 의 **상태 그래프(StateGraph)** 로 모델링하는 프레임워크다. [[LangChain]] 팀이 만들었지만 독립 사용 가능.
- 핵심 아이디어: 에이전트의 흐름을 명시적인 그래프로 표현하면 **분기·반복·병렬·휴먼 인 더 루프** 같은 복잡한 시나리오를 안정적으로 다룰 수 있다.

## 핵심 개념

| 용어 | 의미 |
|------|------|
| `State` | 그래프 전체에서 공유되는 상태(TypedDict, Pydantic) |
| `Node` | 상태를 입력받아 부분 상태를 반환하는 함수 |
| `Edge` | 노드 → 노드 연결. 무조건/조건부 |
| `Conditional Edge` | 함수가 다음 노드를 결정 (LLM 라우팅 등) |
| `Checkpoint` | 상태를 영속화 (DB) — 재시작·HITL 가능 |

## 가장 단순한 ReAct 에이전트

```python
from typing import Annotated, TypedDict
from langgraph.graph import StateGraph, START, END
from langgraph.graph.message import add_messages
from langchain_openai import ChatOpenAI
from langchain_core.messages import HumanMessage

class State(TypedDict):
    messages: Annotated[list, add_messages]  # 메시지 누적 reducer

llm = ChatOpenAI(model="gpt-4o-mini").bind_tools([search_tool])

def call_llm(state: State):
    return {"messages": [llm.invoke(state["messages"])]}

def call_tool(state: State):
    last = state["messages"][-1]
    results = [search_tool.invoke(tc) for tc in last.tool_calls]
    return {"messages": results}

def should_continue(state: State):
    return "tool" if state["messages"][-1].tool_calls else END

graph = (
    StateGraph(State)
    .add_node("llm", call_llm)
    .add_node("tool", call_tool)
    .add_edge(START, "llm")
    .add_conditional_edges("llm", should_continue, {"tool": "tool", END: END})
    .add_edge("tool", "llm")  # 도구 결과를 받아 다시 LLM으로
    .compile()
)

graph.invoke({"messages": [HumanMessage("서울 날씨?")]})
```

## State Reducer 패턴

```python
class State(TypedDict):
    messages: Annotated[list, add_messages]   # 누적 (default는 덮어쓰기)
    counter: Annotated[int, lambda old, new: old + new]
```

- 노드가 부분 상태를 반환하면, reducer가 **기존 값과 어떻게 합칠지** 결정한다.

## 조건부 라우팅 (Supervisor 패턴)

```python
def route(state):
    last_msg = state["messages"][-1].content
    if "결제" in last_msg:   return "payment_agent"
    if "환불" in last_msg:   return "refund_agent"
    return "fallback"

graph.add_conditional_edges("supervisor", route)
```

- 이걸로 [[Supervisor 패턴]] · [[Hierarchical Agent]] 다 구현 가능.

## Checkpointing — 휴먼 인 더 루프

```python
from langgraph.checkpoint.memory import MemorySaver
graph = builder.compile(checkpointer=MemorySaver())

config = {"configurable": {"thread_id": "user-42"}}
graph.invoke(input, config=config)

# 중간 상태에서 사용자 승인 → 같은 thread_id로 재개
graph.invoke(None, config=config)
```

- DB(`SqliteSaver`, `PostgresSaver`)로 바꾸면 영속화. 장기 실행·복구·다중 세션이 자연스러워진다.

## LangGraph가 빛나는 시나리오

- **반복(loop)** 이 있는 에이전트 — ReAct, Reflection, 재시도.
- **여러 [[Multi Agent|에이전트]]가 협업** — Supervisor, Swarm.
- **휴먼 승인 단계** 가 필요한 워크플로우.
- 같은 작업을 여러 번 **재현**하거나 **시각화**해 디버깅해야 할 때 (LangGraph Studio).

## 한계

- 학습 곡선이 LangChain보다 가파름.
- 단순 1-shot 호출엔 오버엔지니어링.

## 관련

- [[LangChain]] — 컴포넌트 계층.
- [[Supervisor 패턴]] · [[Swarm 패턴]] · [[Hierarchical Agent]] — LangGraph로 구현 가능한 멀티 에이전트 토폴로지.
