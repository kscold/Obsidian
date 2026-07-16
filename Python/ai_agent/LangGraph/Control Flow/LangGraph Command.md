---
title: LangGraph Command
created: 2026-07-15
tags:
  - ai-agent
  - langgraph
  - state
  - control-flow
---

# LangGraph Command

`Command`는 LangGraph 노드가 **state를 갱신하면서 다음 실행 위치까지 함께 결정**하게 하는 반환값이다. 일반 노드는 `dict`만 반환하고 다음 노드는 edge가 정하지만, `Command`는 한 판단 결과에서 업데이트와 라우팅을 원자적으로 표현한다.

## 기본 문법

```python
from typing import Literal
from langgraph.types import Command

def review_node(state) -> Command[Literal["plan", "respond"]]:
    if state["needs_revision"]:
        return Command(
            update={"review_reason": "필수 정보가 부족함"},
            goto="plan",
        )
    return Command(
        update={"review_reason": "검토 통과"},
        goto="respond",
    )
```

- `update`: [[LangGraph State|State]]에 적용할 부분 업데이트다. reducer가 설정된 필드는 그 reducer를 거친다.
- `goto`: 다음 노드 이름 또는 노드 이름 목록이다.
- 반환 타입의 `Literal`은 실제로 갈 수 있는 노드를 타입으로 문서화한다.

## Conditional Edge와의 구분

| 방식 | 적합한 경우 | 장점 |
|---|---|---|
| `add_conditional_edges` | 분기 규칙을 그래프 정의에 모으고 싶을 때 | topology를 한눈에 보기 쉽다. |
| `Command(goto=...)` | 노드의 판단 결과와 state 갱신이 한 덩어리일 때 | 갱신과 이동의 불일치를 줄인다. |

둘을 섞을 수는 있지만, 같은 결정에 두 방식을 중복하면 흐름을 추적하기 어려워진다. 고정된 프로세스 분기는 edge에, 동적으로 계산한 handoff나 재계획 결과는 `Command`에 두는 식으로 경계를 정한다.

## 도구에서 State를 갱신할 때

도구가 단순 조회 결과가 아니라 에이전트 state를 바꿔야 하면 `Command`를 반환할 수 있다.

```python
from langchain.messages import ToolMessage
from langchain.tools import ToolRuntime, tool
from langgraph.types import Command

@tool
def set_language(language: str, runtime: ToolRuntime) -> Command:
    """응답 언어를 변경한다."""
    return Command(
        update={
            "language": language,
            "messages": [
                ToolMessage(
                    content=f"언어를 {language}로 설정했습니다.",
                    tool_call_id=runtime.tool_call_id,
                )
            ],
        }
    )
```

이 경우 도구 반환값의 의미가 "텍스트 결과"에서 "state transition"으로 바뀐다. 어떤 도구가 state를 쓰는지 명확히 구분하고, 병렬 실행 가능 여부를 함께 검토해야 한다.

## `resume`은 다른 용도다

`Command(resume=value)`은 [[LangGraph interrupt]] 후 그래프를 재개할 때만 `invoke`나 `stream`의 입력으로 사용한다.

```python
graph.invoke(Command(resume={"approved": True}), config=config)
```

반대로 `Command(update=...)`나 `Command(goto=...)`는 보통 **노드 또는 도구의 반환값**이다. 완료된 그래프에 이를 입력으로 넣어 다음 턴을 시작하면 마지막 checkpoint에서 재개되어 의도와 다른 위치에서 실행될 수 있다. 새 사용자 턴은 일반 state 입력 dict로 시작한다.

## 주의점

- `goto` 대상은 그래프에 실제로 등록돼 있어야 한다.
- `update`는 전체 state를 복사하지 말고, 해당 노드가 새로 만든 변경만 반환한다.
- subgraph에서 부모 graph로 이동할 때는 `graph=Command.PARENT`와 부모 state reducer를 함께 설계한다.
- `Command`가 흐름을 숨기지 않도록, 동적 라우팅 사유를 [[Observability|trace]]에 남긴다.

## 관련

- [[LangGraph State]]
- [[LangGraph State Reducer]]
- [[LangGraph Edge]]
- [[LangGraph interrupt]]
- [[LangGraph ToolNode]]
- [[Concurrent Tool Execution]]

## 공식 문서

- [LangGraph Graph API: Command와 Send](https://docs.langchain.com/oss/python/langgraph/use-graph-api)
- [LangChain Tools: Command로 state 갱신](https://docs.langchain.com/oss/python/langchain/tools)
