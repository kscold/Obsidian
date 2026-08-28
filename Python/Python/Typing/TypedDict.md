---
title: TypedDict
created: 2026-06-26
tags:
  - python
  - typing
---

# TypedDict

`TypedDict`는 dict의 key와 value 타입을 타입 힌트로 선언하는 방법이다.

런타임 객체는 그냥 dict지만, 타입 체커와 IDE가 어떤 key가 있는지 알 수 있다.

## 예

```python
from typing import TypedDict

class WorkflowAgentState(TypedDict):
    user_request: str
    metadata: list[dict]
    actions: list[dict]
```

## total=False

```python
class WorkflowResponse(TypedDict, total=False):
    message: str
    actions: list[dict]
```

`total=False`면 모든 key가 선택값이 된다. 응답 payload처럼 일부 필드만 채울 수 있는 dict에 좋다.

## LangGraph에서의 역할

[[LangGraph State]]는 노드 간에 주고받는 dict이다. `TypedDict`를 쓰면 state의 필드 구조를 코드로 문서화할 수 있다.

## 한 줄 정리

`TypedDict`는 **dict를 그대로 쓰면서 key 구조를 타입으로 설명하는 방법**이다.

## 관련

- [[Annotated]]
- [[LangGraph State Reducer]]
- [[typing 모듈]]
