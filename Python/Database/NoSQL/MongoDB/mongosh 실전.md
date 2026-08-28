---
title: mongosh 실전
created: 2026-08-28
tags:
  - database
  - mongodb
  - operations
---

# mongosh 실전

- `mongosh`는 MongoDB 셸이다. **개념을 눈으로 확인하는 가장 빠른 방법**이고, 장애 시 첫 번째 진단 도구다.
- 셸 문법은 JavaScript다. 파이썬 드라이버와 이름이 **camelCase / snake_case로 다르다**.

| 셸 (JS) | [[motor]] (Python) |
|---|---|
| `findOne()` | `find_one()` |
| `insertOne()` | `insert_one()` |
| `replaceOne()` | `replace_one()` |
| `updateMany()` | `update_many()` |
| `countDocuments()` | `count_documents()` |
| `createIndex()` | `create_index()` |
| `getIndexes()` | `list_indexes()` |

## 접속과 둘러보기

```javascript
// mongosh "mongodb://localhost:27017/ai_agent"
show dbs
use ai_agent
show collections

db.blocks.countDocuments()
db.blocks.findOne()                       // 문서 한 건 — 실제 모양 확인
db.blocks.find().limit(3).pretty()
```

**스키마가 없으니 `findOne()`이 곧 스키마 문서다.** 새 컬렉션을 만났을 때 제일 먼저 치는 명령.

## 실제로 자주 쓰는 조회

```javascript
// 특정 블록
db.blocks.findOne({ block_id: "TSM0004" }, { _id: 0 })

// 카테고리별 개수 — 데이터 분포 파악
db.blocks.aggregate([{ $group: { _id: "$category", n: { $sum: 1 } } }, { $sort: { n: -1 } }])

// 시드 출처별
db.blocks.countDocuments({ _origin: "python_seed" })
db.blocks.countDocuments({ _origin: "swarm_consensus" })

// 최근 실행 원장
db.agent_trace.find({}, { _id: 0, node_name: 1, ts_start: 1 }).sort({ ts_start: -1 }).limit(10)

// 시드 배포 영수증 — "지금 어떤 버전이 깔려 있나"
db.catalog_seed_deployments.findOne({ seed_key: "workflow_catalog" })
```

## 인덱스 진단

```javascript
db.blocks.getIndexes()
db.blocks.find({ category: "visualization" }).explain("executionStats")
```

읽는 곳은 두 군데다 ([[explain과 실행 계획]]).

```text
winningPlan.stage         IXSCAN(좋음) / COLLSCAN(인덱스 없음)
executionStats
  ├─ nReturned            돌려준 문서 수
  └─ totalDocsExamined    확인한 문서 수   ← 이 둘이 비슷해야 좋은 쿼리
```

```javascript
db.blocks.stats()                 // 문서 수, 데이터 크기, 인덱스 크기
db.blocks.totalIndexSize()
```

## 위험한 명령 — 실행 전에 한 번 더

```javascript
db.blocks.deleteMany({})          // 전부 삭제. 되돌릴 수 없다
db.blocks.drop()                  // 컬렉션 자체 삭제 (인덱스까지)
db.blocks.updateMany({}, { $set: {...} })   // 필터 없는 갱신 = 전체
```

> **필터를 빠뜨린 `deleteMany`/`updateMany`가 운영 사고 1순위다.**
> 습관: 지우기 전에 같은 필터로 `countDocuments()`를 먼저 친다.

```javascript
db.blocks.countDocuments({ _origin: "python_seed", block_id: { $nin: seedIds } })  // 확인
db.blocks.deleteMany({ _origin: "python_seed", block_id: { $nin: seedIds } })      // 그 다음
```

## 셸에서 개념 확인하기

배운 걸 눈으로 보는 순서다.

| 확인할 개념 | 명령 |
|---|---|
| [[풀 스캔(Full Scan)]] | 인덱스 없는 필드로 `explain` → `COLLSCAN` |
| [[유니크 인덱스]] | 같은 `block_id`를 두 번 `insertOne` → `E11000 duplicate key` |
| [[upsert]] | 같은 `replaceOne(..., {upsert:true})` 두 번 → 문서 수 그대로 |
| [[MongoDB 배열 쿼리]] | `$elemMatch` 있는 것과 없는 것의 결과 차이 |
| [[MongoDB 인덱스 실전\|TTL]] | `getIndexes()`에서 `expireAfterSeconds` 확인 |

## 한 줄 정리

`findOne()`으로 모양을 보고, `explain()`으로 인덱스를 보고, **지우기 전에 `countDocuments()`로 센다.**

## 관련

- [[MongoDB CRUD 메서드]]
- [[explain과 실행 계획]]
- [[MongoDB 인덱스 실전]]
- [[AI Agent 컬렉션 지도]]
- [[Seed 배포와 멱등 동기화]]
- [[MongoDB MOC]]
