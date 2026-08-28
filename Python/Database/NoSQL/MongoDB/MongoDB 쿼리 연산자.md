---
title: MongoDB 쿼리 연산자
created: 2026-08-28
tags:
  - database
  - mongodb
  - query
---

# MongoDB 쿼리 연산자

[[MongoDB]] 필터는 JSON이다. 키가 필드, 값이 조건이고 `$`로 시작하는 게 연산자다.

## 자주 쓰는 것

| 연산자 | 뜻 | 예 |
|---|---|---|
| (값만) | 같다 (`$eq`) | `{ status: "ACTIVE" }` |
| `$ne` | 다르다 | `{ status: { $ne: "archived" } }` |
| `$in` `$nin` | 목록에 있다 / 없다 | `{ category: { $in: ["model", "evaluate"] } }` |
| `$gt` `$gte` `$lt` `$lte` | 대소 비교 | `{ created_at: { $gte: since } }` |
| `$exists` | 필드 존재 여부 | `{ summary: { $exists: true } }` |
| `$regex` | 정규식 | `{ name: { $regex: "^TSM" } }` |
| `$and` `$or` `$not` | 논리 결합 | `{ $or: [ {...}, {...} ] }` |

## 최상위 여러 조건은 자동 AND

```javascript
db.blocks.find({ category: "model", is_deprecated: false })
// = category가 model 이고 AND is_deprecated가 false
```

`$or`가 필요할 때만 명시적으로 쓴다.

## 주의

- `$regex`는 앞이 고정된 패턴(`^TSM`)이 아니면 [[인덱스(Index)|인덱스]]를 못 탄다. 텍스트 검색 대용으로 쓰면 안 된다. 그 자리는 [[BM25]]나 [[Qdrant]]의 일이다.
- `$ne`, `$nin`도 인덱스 효율이 나쁘다. "아닌 것"보다 "인 것"으로 조건을 뒤집을 수 있는지 먼저 본다.

## 한 줄 정리

Mongo 필터는 **JSON으로 쓴 WHERE 절**이고, `$regex`·`$ne`는 인덱스를 못 타니 조심한다.

## 관련

- [[MongoDB]]
- [[projection]]
- [[인덱스(Index)]]
- [[Qdrant 메타데이터 필터]]
