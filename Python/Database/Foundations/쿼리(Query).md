---
title: 쿼리(Query)
created: 2026-08-28
tags:
  - database
  - query
---

# 쿼리(Query)

- 쿼리는 **데이터에게 던지는 질문**이다.
- 핵심 성질은 **선언형**이라는 것. "어떻게 찾아라"가 아니라 "무엇을 원한다"만 쓴다. 찾는 방법은 DB의 옵티마이저가 정한다.

## 같은 질문, 세 언어

"카테고리가 시각화인 블록 10개"

```sql
-- SQL
SELECT block_id, name FROM blocks WHERE category = 'visualization' LIMIT 10;
```

```javascript
// MongoDB — JSON 그 자체가 질문이다
db.blocks.find({ category: "visualization" }, { block_id: 1, name: 1 }).limit(10)
```

```python
# Qdrant — 조건이 아니라 "이 벡터와 가까운 것"이 질문이다
client.query_points(collection_name="block-catalog-bge-m3", query=vec, limit=10)
```

## 두 종류의 질문

| 종류 | 답의 모양 | 예 |
|---|---|---|
| **조건이 참인 것** | 집합. 정답이 하나로 정해짐 | [[SQL]], [[MongoDB]] |
| **가장 가까운 것** | 순위. 정답이 상대적 | [[Qdrant]], [[BM25]] |

이 차이가 나머지 전부를 가른다. 앞은 "맞다/틀리다"고, 뒤는 "1등이 진짜 1등인가"라는 [[Recall과 ef 튜닝|recall]] 문제가 된다.

## 한 줄 정리

쿼리는 **원하는 결과만 선언하는 질문**이고, 조건형(SQL·Mongo)과 유사도형(벡터·BM25)으로 나뉜다.

## 관련

- [[SQL]]
- [[MongoDB]]
- [[Qdrant]]
- [[인덱스(Index)]]
