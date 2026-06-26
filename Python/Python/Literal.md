---
title: Literal
created: 2026-06-26
tags:
  - python
  - typing
---

# Literal

`typing.Literal`은 변수나 인자가 특정 값들 중 하나여야 한다는 타입 힌트이다.

```python
from typing import Literal

RowCountBand = Literal["tiny", "small", "medium", "large"]
```

## 언제 쓰나

값의 종류가 정해진 문자열/숫자 집합일 때 쓴다. 단순 `str`보다 좁은 타입이므로 IDE 자동완성, 정적 검사, 리팩터링에 도움이 된다.

## 실무에서의 쓰임

데이터/agent 상태 모델에서는 `RowCountBand`, `ProblemType` 같은 도메인 라벨을 `Literal`로 정의할 수 있다. 이렇게 하면 `"classification"`, `"regression"`, `"clustering"`, `"eda"`처럼 허용되는 problem type이 코드에 명시된다.

관련: [[TypedDict]], [[Protocol]], [[dataclass]]
