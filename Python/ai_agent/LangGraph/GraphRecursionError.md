---
title: GraphRecursionError
created: 2026-06-23
tags:
  - langgraph
  - error
  - loop-control
---

# GraphRecursionError

- `GraphRecursionError`는 [[LangGraph]] 그래프가 허용된 반복 제한을 넘었을 때 발생하는 에러다.
- 보통 [[LangGraph recursion_limit]]에 도달했을 때 나온다.
- 이 에러는 "정상 종료"가 아니라 "너무 많이 반복되어 강제로 멈췄다"는 신호다.

## 언제 발생하나

- 그래프에 [[END]]로 가는 edge가 없을 때
- 같은 노드로 계속 돌아가는 edge가 있을 때
- 라우터 함수가 계속 `"loop"`만 반환할 때
- agent가 같은 tool call을 반복할 때
- 조건부 종료 로직이 잘못 작성되었을 때

## 예시

```python
from langgraph.errors import GraphRecursionError

try:
    graph.invoke({"count": 0}, {"recursion_limit": 10})
except GraphRecursionError:
    print("recursion_limit 도달, 에러 중단")
```

## 확인할 것

- [[LangGraph Edge]]가 [[END]]로 이어지는지 확인한다.
- `add_conditional_edges`의 라우터 함수가 종료 조건을 반환하는지 확인한다.
- State 값이 반복 중 실제로 변하는지 확인한다.
- 의도한 반복 횟수보다 `recursion_limit`이 너무 낮지 않은지 확인한다.

## 한 줄 요약

- `GraphRecursionError` = 그래프가 정상 종료하지 못하고 반복 제한에 걸려 멈춘 에러.
- 해결은 `recursion_limit`을 무작정 키우는 것이 아니라 종료 조건과 edge 구조를 확인하는 것이다.

## 관련

- [[LangGraph recursion_limit]]
- [[Loop Control]]
- [[LangGraph Edge]]
