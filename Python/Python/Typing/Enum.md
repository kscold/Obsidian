---
title: Enum
created: 2026-08-28
tags:
  - python
  - typing
---

# Enum

- `Enum`은 **정해진 값들의 집합에 이름을 붙이는** 타입이다. 문자열 상수가 코드 곳곳에 흩어지는 걸 막는다.

```python
from enum import Enum

class GraphCompileStatus(str, Enum):
    OK = "ok"
    DEGRADED = "degraded"
    FAILED = "failed"
```

## `str, Enum`을 상속하는 이유

```python
class AuditScope(str, Enum):
    PROMPT = "prompt"
```

`str`을 함께 상속하면 **문자열처럼 쓸 수 있으면서 Enum이기도 하다.**

```python
AuditScope.PROMPT == "prompt"          # True
json.dumps({"scope": AuditScope.PROMPT})   # 그냥 직렬화됨
f"{AuditScope.PROMPT}"                 # 파이썬 버전에 따라 표기가 다르니 .value 를 쓰는 게 안전
```

순수 `Enum`이면 `.value`를 매번 꺼내야 하고, JSON 직렬화에서 걸린다. **DB에 넣거나 API로 나가는 값은 거의 항상 `str, Enum`** 이다.

> 파이썬 3.11+에는 같은 목적의 `StrEnum`이 있다. 3.10 이하 호환이 필요하면 `str, Enum`을 쓴다.

## Enum vs Literal

| | `Enum` | [[Literal]] |
|---|---|---|
| 실체 | 런타임 객체 | 타입 힌트뿐 |
| 값 목록 순회 | `list(MyEnum)` 가능 | 불가 |
| 메서드 부착 | 가능 | 불가 |
| [[Pydantic]] 스키마 | `enum`으로 변환됨 | `enum`으로 변환됨 |
| LLM 출력 스키마 | 무겁다 | **가볍고 충분** |

**결론**: 값에 동작이 붙거나 목록을 순회해야 하면 `Enum`, 단순히 "이 중 하나"만 표현하면 `Literal`이 낫다. [[Structured Output]] 스키마는 대부분 `Literal`로 충분하다.

## 유용한 것들

```python
class CollectionOrigin(str, Enum):
    SEED = "python_seed"
    ADMIN = "admin"
    SWARM = "swarm_consensus"

list(CollectionOrigin)                     # 전체 목록 — 검증·문서화에 유용
CollectionOrigin("admin")                  # 값 → 멤버 (없으면 ValueError)
CollectionOrigin.SEED.value                # "python_seed"
CollectionOrigin.SEED.name                 # "SEED"
```

```python
try:
    origin = CollectionOrigin(raw_value)
except ValueError:
    origin = CollectionOrigin.SEED         # 알 수 없는 값 → 명시적 기본
```

**외부에서 들어온 문자열을 Enum으로 바꾸는 지점이 곧 검증 지점**이다. 여기를 통과하면 그 뒤로는 오타가 있을 수 없다.

## 언제 쓰나

- 상태 머신의 상태 (`ok` / `degraded` / `failed`)
- 출처·종류 태그 (`python_seed` / `admin`)
- 값에 **의미가 붙고 코드 여러 곳에서 비교**될 때

반대로 한 함수 안에서만 쓰는 값이면 그냥 문자열이 낫다. Enum도 비용이다.

## 한 줄 정리

Enum은 **흩어진 문자열 상수를 한곳에 모으는 타입**이고, DB·JSON을 넘나든다면 `str, Enum`으로 만든다.

## 관련

- [[Literal]]
- [[typing 모듈]]
- [[Pydantic v2 메서드 사전]]
- [[Structured Output]]
- [[dataclass]]
