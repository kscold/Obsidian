---
title: field(default_factory)
created: 2026-06-26
tags:
  - python
  - dataclass
---

# field(default_factory)

`field(default_factory=...)`는 [[dataclass]]에서 list나 dict 같은 mutable 기본값을 안전하게 만들 때 사용한다.

## 나쁜 예

```python
@dataclass
class Report:
    violations: list = []
```

이렇게 하면 모든 인스턴스가 같은 list를 공유할 수 있다.

## 좋은 예

```python
from dataclasses import dataclass, field

@dataclass
class Report:
    violations: list = field(default_factory=list)
```

인스턴스를 만들 때마다 새 list가 생성된다.

## 언제 쓰나

- `list`
- `dict`
- `set`
- 동적으로 만든 기본 객체

## 한 줄 정리

`field(default_factory)`는 **dataclass에서 mutable 기본값 공유 버그를 막는 생성 함수 지정 방식**이다.

## 관련

- [[dataclass]]
- [[PipelineContractReport]]
