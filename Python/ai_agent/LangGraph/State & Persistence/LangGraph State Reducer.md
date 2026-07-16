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

LangGraph State Reducer는 여러 노드가 같은 state 필드를 업데이트할 때 값을 어떻게 합칠지 정하는 함수이다. reducer가 없으면 해당 키의 새 값이 기존 값을 덮어쓴다.

에이전트 state에는 메시지, 검색 근거, 작업 결과처럼 누적해야 하는 값과 현재 계획처럼 교체해야 하는 값이 섞여 있다. reducer는 이 둘의 의미를 state 스키마에 명시하는 장치다.

## Annotated 사용

```python
class WorkflowAgentState(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages_reducer]
    actions: Annotated[list[dict], operator.add]
```

- `messages`는 커스텀 함수로 합친다.
- `actions`는 `operator.add`로 리스트를 이어붙인다.

## reducer의 계약

좋은 reducer는 다음 성질을 가진다.

- 입력 `left`, `right`를 직접 변경하지 않는 순수 함수다.
- 같은 update를 재적용해도 예상 가능한 결과를 낸다.
- 병렬 fan-out이 있을 때 실행·도착 순서에 결과가 의존하지 않는다.
- 재개·replay에서 같은 state를 복원할 수 있다.

특히 여러 worker가 같은 키를 갱신하는 [[LangGraph Send]] 구조에서는 `operator.add`처럼 결합 순서에 안전한 reducer가 필요하다. 마지막 값을 덮어쓰는 기본 reducer를 그대로 쓰면 worker 결과 일부가 사라질 수 있다.

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

실무에서는 단순 리스트 덧붙이기보다 LangGraph의 `add_messages` 계열 reducer가 더 안전한 경우가 많다. 새 메시지는 추가하면서도 같은 message ID의 수정은 교체해, human edit나 tool 결과 재반영을 다룰 수 있다.

## 새 턴과 누적을 구분한다

checkpointer를 쓰면 같은 `thread_id`의 이전 state가 다음 입력과 병합될 수 있다. 따라서 "턴 안에서는 누적, 새 턴에서는 초기화"할 필드는 명시적인 reset 규칙이 필요하다.

- 대화 이력은 보통 누적하거나 요약한다.
- 현재 턴의 action·임시 오류·작업 결과는 새 턴에 비운다.
- 장기 선호는 [[LangGraph Store]] 또는 별도 memory에 둔다.

reset marker나 `turn_id`를 이용한 커스텀 reducer를 만들 수 있지만, 순서 의존 규칙은 replay·병렬 상황을 포함해 테스트해야 한다.

## 주의점

- tool call 메시지는 원자적 그룹으로 유지해야 한다.
- `AIMessage(tool_calls)`와 해당 `ToolMessage`가 분리되면 OpenAI API 규칙에 걸릴 수 있다.
- 오래된 tool 메시지는 요약해 토큰을 줄이는 것이 좋다.
- reducer는 validation이나 외부 I/O를 수행하면 안 된다. state 병합만 담당해야 한다.

## 한 줄 정리

LangGraph State Reducer는 **노드들이 같은 state 필드를 갱신할 때 덮어쓸지, 누적할지, 교체할지를 정하는 병합 규칙**이다.

## 관련

- [[Annotated]]
- [[TypedDict]]
- [[LangGraph State]]
- [[LangGraph Agent Loop]]
- [[Tool Calling]]
- [[LangGraph Send]]
- [[LangGraph Command]]
- [[LangGraph Durable Execution]]

## 공식 문서

- [LangGraph State와 reducer](https://docs.langchain.com/oss/python/langgraph/graph-api)
