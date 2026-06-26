---
title: str.removeprefix
created: 2026-06-26
tags:
  - python
  - method
---

# str.removeprefix

`str.removeprefix(prefix)`는 문자열이 특정 prefix로 시작하면 그 prefix만 제거하고, 아니면 원문을 그대로 반환한다.

```python
reason = reason.removeprefix("[NO_OPTION_CHANGE]").strip()
```

## 실무에서의 쓰임

[[Response Builder Pattern]]에서는 내부용 prefix나 routing marker를 사용자 응답에서 제거할 때 `removeprefix`를 쓸 수 있다. 내부 marker는 로그와 분기에는 필요하지만, 사용자에게 그대로 노출되면 안 된다.

관련: [[No-op Change Guard]]
