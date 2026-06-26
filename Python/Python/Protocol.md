---
title: Protocol
created: 2026-06-26
tags:
  - python
  - typing
---

# Protocol

`Protocol`은 "이 메서드와 속성을 가진 객체라면 같은 타입으로 보겠다"는 구조적 타입 힌트이다.

상속 관계보다 인터페이스 모양을 중요하게 본다. 다른 언어의 interface와 비슷하지만, Python스럽게 duck typing을 타입 힌트로 표현한다.

## 예

```python
from typing import Protocol

class SubAgent(Protocol):
    name: str

    async def run(self, payload) -> object:
        ...
```

`SubAgent`를 상속하지 않아도 `name`과 `run()`이 있으면 타입 체커는 SubAgent처럼 취급할 수 있다.

## runtime_checkable

```python
from typing import runtime_checkable

@runtime_checkable
class SubAgent(Protocol):
    ...
```

이렇게 하면 런타임에서도 `isinstance(obj, SubAgent)` 검사가 가능해진다.

## 언제 쓰나

- 플러그인 구조
- sub-agent 공통 계약
- 테스트 double/mock
- 상속을 강제하고 싶지 않은 인터페이스

## 한 줄 정리

`Protocol`은 **상속보다 객체의 모양을 기준으로 타입 계약을 표현하는 Python 타입 힌트**이다.

## 관련

- [[typing 모듈]]
- [[dataclass]]
- [[Fail-soft]]
