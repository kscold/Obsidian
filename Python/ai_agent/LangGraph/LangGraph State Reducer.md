---
title: LangGraph State Reducer
created: 2026-06-26
tags:
  - ai-agent
  - langgraph
  - state
  - python
---

# LangGraph State Reducer

LangGraph State Reducer는 여러 노드가 같은 state 필드를 업데이트할 때 값을 어떻게 합칠지 정하는 함수이다.

프로젝트에서는 `WorkflowAgentState`의 `messages`, `actions` 같은 필드에 reducer를 붙여 노드 출력이 누적되도록 만든다.

## Annotated 사용

```python
class WorkflowAgentState(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages_reducer]
    actions: Annotated[list[dict], operator.add]
```

- `messages`는 커스텀 함수로 합친다.
- `actions`는 `operator.add`로 리스트를 이어붙인다.

## 왜 필요할까

일반 dict 업데이트는 같은 키를 덮어쓴다. 하지만 에이전트 상태에서는 메시지나 action이 누적되어야 한다.

```text
기존 messages + 새 ToolMessage -> 전체 messages
```

## 메시지 reducer 예

`SystemMessage`로 시작하면 전체 교체하고, 그 외에는 뒤에 추가한다.

```python
def add_messages_reducer(left, right):
    if right and isinstance(right[0], SystemMessage):
        return right
    return left + right
```

## 주의점

- tool call 메시지는 원자적 그룹으로 유지해야 한다.
- `AIMessage(tool_calls)`와 해당 `ToolMessage`가 분리되면 OpenAI API 규칙에 걸릴 수 있다.
- 오래된 tool 메시지는 요약해 토큰을 줄이는 것이 좋다.

## 한 줄 정리

LangGraph State Reducer는 **노드들이 같은 state 필드를 갱신할 때 덮어쓸지, 누적할지, 교체할지를 정하는 병합 규칙**이다.

## 관련

- [[Annotated]]
- [[TypedDict]]
- [[LangGraph State]]
- [[LangGraph Agent Loop]]
- [[Tool Calling]]
