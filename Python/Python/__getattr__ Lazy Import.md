---
title: __getattr__ Lazy Import
created: 2026-06-26
tags:
  - python
  - import
  - pattern
---

# __getattr__ Lazy Import

모듈 수준 `__getattr__`는 `module.symbol` 접근이 들어왔을 때 동적으로 값을 반환할 수 있게 해준다.

무거운 의존성을 import 시점에 바로 로드하지 않고, 실제로 필요한 순간에만 import하는 [[Lazy Initialization]] 패턴으로 사용할 수 있다.

## 예

```python
def __getattr__(name: str):
    if name == "WorkflowAgent":
        from ai_agent.workflow.graph import agent
        return getattr(agent, name)
    raise AttributeError(name)
```

## 왜 쓰나

- LangChain/LangGraph import가 무겁다.
- 순환 import를 피하고 싶다.
- 패키지의 public API는 유지하면서 내부 로딩을 늦추고 싶다.

## 주의점

- 존재하지 않는 이름은 반드시 `AttributeError`를 raise해야 한다.
- 너무 남용하면 import 흐름이 숨는다.
- 타입 체커가 자동으로 이해하지 못할 수 있다.

## 한 줄 정리

`__getattr__` Lazy Import는 **모듈 심볼 접근 시점까지 무거운 import를 미루는 패턴**이다.

## 관련

- [[Lazy Initialization]]
- [[LangGraph Agent Loop]]
