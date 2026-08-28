---
title: MongoDB find()
created: 2026-08-28
tags:
  - database
  - mongodb
  - method
---

# MongoDB find()

**여러 문서를 조회한다. 결과가 아니라 [[MongoDB 커서와 페이지네이션|커서]]를 돌려준다.**

```javascript
db.<컬렉션>.find( <filter>, <projection> )
```

| 인자 | 뜻 | 생략 시 |
|---|---|---|
| `filter` | 조건 JSON ([[MongoDB 쿼리 연산자]]) | `{}` = 전체 |
| `projection` | 받을 필드 ([[projection]]) | 전 필드 |

## 셸

```javascript
db.blocks.find()                                   // 전체
db.blocks.find({ category: "model" })
db.blocks.find({ category: "model" }, { _id: 0, block_id: 1, name: 1 })
db.blocks.find({ score: { $gte: 0.5 } }).sort({ score: -1 }).limit(10)
```

## 파이썬 ([[motor]])

```python
cursor = db.blocks.find({}, {"_id": 0})      # await 없음 — 커서만 만든다
async for doc in cursor:                     # 여기서 실제로 통신
    ...

docs = await db.blocks.find(query, {"_id": 0}).to_list(length=200)
```

## 반환

- **셸**: 커서. 자동으로 처음 20건을 출력하고 `it`로 더 본다.
- **파이썬**: `AsyncIOMotorCursor`. `async for` 또는 `to_list(length=N)`으로 소비.

## 자주 틀리는 것

- `await db.blocks.find(...)` — **커서에 await를 걸면 안 된다.** 소비 시점에 건다.
- `to_list(length=None)`은 전부 메모리에 올린다. 항상 상한을 준다.
- 커서는 기본 10분 뒤 서버에서 만료된다. 오래 붙잡고 다른 일을 하면 `CursorNotFound`가 난다.
- 결과가 없으면 **빈 커서**다. 예외가 아니다.

## 체인 메서드

`.sort()` · `.limit()` · `.skip()` · `.projection()` — [[MongoDB sort() limit() skip()]]

## 한 줄 정리

`find(filter, projection)`은 **커서를 만들 뿐**이고, 소비할 때 batch 단위로 흐른다.

## 관련

- [[MongoDB findOne()]]
- [[MongoDB sort() limit() skip()]]
- [[MongoDB 커서와 페이지네이션]]
- [[MongoDB 쿼리 연산자]]
- [[projection]]
- [[MongoDB CRUD 메서드]]
