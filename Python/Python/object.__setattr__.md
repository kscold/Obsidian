---
title: object.__setattr__
created: 2026-06-26
tags:
  - python
  - oop
  - advanced
---

# object.__setattr__

`object.__setattr__`는 객체의 기본 attribute 설정 메서드를 직접 호출하는 방법이다.

일반 `setattr(obj, name, value)`가 클래스의 `__setattr__` 검증에 막힐 때 우회가 필요할 수 있다.

## 예

```python
object.__setattr__(tool, "invoke", wrapped_invoke)
```

프로젝트에서는 LangChain `StructuredTool`이 Pydantic 모델이라 `extra=forbid` 검증에 막힐 수 있어 이 방식을 사용한다.

## 왜 조심해야 하나

이 방식은 클래스가 의도한 검증을 우회한다.

따라서 다음처럼 제한된 목적에만 써야 한다.

- observability wrapper 삽입
- 테스트 fixture 구성
- 불변 객체 내부 구현

## 한 줄 정리

`object.__setattr__`는 **객체의 attribute 설정 검증을 우회해 직접 값을 넣는 고급 Python 메서드**이다.

## 관련

- [[Tool Observability Wrapping]]
- [[Pydantic]]
- [[functools.wraps]]
