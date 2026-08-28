---
title: collections.defaultdict
created: 2026-08-28
tags:
  - python
  - stdlib
---

# collections.defaultdict

- `defaultdict`는 **없는 키를 읽으면 기본값을 자동으로 만들어 주는 dict**다.
- 누적·집계 코드에서 `if key not in d` 분기를 통째로 없앤다.

```python
from collections import defaultdict

scores = defaultdict(float)     # 없는 키 → 0.0
scores["TSM0004"] += 0.016      # KeyError 안 남
```

## 실제 사용 — [[Reciprocal Rank Fusion|RRF]] 누적

```python
def rrf_merge(ranked_lists: list[list[str]], rrf_k: int = 60, top_n=None) -> list[str]:
    scores: dict[str, float] = defaultdict(float)
    first_seen: dict[str, int] = {}
    sequence = 0
    for ranked in ranked_lists:
        for rank, item in enumerate(ranked, start=1):
            block_id = item[0] if isinstance(item, tuple) else item
            if block_id not in first_seen:
                first_seen[block_id] = sequence
                sequence += 1
            scores[block_id] += 1.0 / (rrf_k + rank)      # 초기화 코드가 필요 없다
    ...
```

`defaultdict(float)`가 없으면 매번 이렇게 써야 한다.

```python
if block_id not in scores:
    scores[block_id] = 0.0
scores[block_id] += 1.0 / (rrf_k + rank)
```

## 기본값 팩토리

```python
defaultdict(int)      # 0        — 카운팅
defaultdict(float)    # 0.0      — 점수 누적
defaultdict(list)     # []       — 그룹핑
defaultdict(set)      # set()    — 중복 없는 그룹핑
defaultdict(dict)     # {}       — 중첩 구조
```

인자는 **호출 가능한 것**이다. 값이 아니라 팩토리라서 매번 새 객체가 만들어진다 — [[field(default_factory)]]와 같은 이유.

## 함정 둘

**1) 읽기만 해도 키가 생긴다.**

```python
d = defaultdict(list)
if d["없는키"]:        # 조회했을 뿐인데
    ...
print(len(d))          # 1 — 키가 만들어졌다
```

조회만 하고 싶으면 `d.get(key)`를 쓴다.

**2) 순회 중 조회하면 크기가 바뀐다.** `RuntimeError: dictionary changed size during iteration`이 난다.

## 형제들

| 클래스 | 쓰임 |
|---|---|
| `defaultdict` | 없는 키 자동 초기화 |
| `Counter` | 개수 세기 전용. `most_common()` 제공 |
| `OrderedDict` | 순서 조작 — [[OrderedDict와 LRU 캐시]] |
| `deque` | 양쪽 끝 O(1) 큐 |

## 한 줄 정리

`defaultdict`는 **초기화 분기를 없애는 dict**이고, 조회만으로도 키가 생긴다는 것만 조심하면 된다.

## 관련

- [[Reciprocal Rank Fusion]]
- [[OrderedDict와 LRU 캐시]]
- [[field(default_factory)]]
- [[typing 모듈]]
