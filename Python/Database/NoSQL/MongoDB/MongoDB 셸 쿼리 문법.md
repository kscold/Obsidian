---
title: MongoDB 셸 쿼리 문법
created: 2026-08-28
tags:
  - database
  - mongodb
  - syntax
---

# MongoDB 셸 쿼리 문법

[[SQL]]이 문장이라면 MongoDB는 **메서드 호출 + JSON 인자**다. 문법의 전부가 이 한 줄에 있다.

```javascript
db.<컬렉션>.<메서드>( <필터 JSON>, <옵션 JSON> )
```

## 조회

```javascript
db.blocks.find()                                   // 전부
db.blocks.find({ category: "model" })              // 필터
db.blocks.find({ category: "model" }, { _id: 0, block_id: 1, name: 1 })   // projection
db.blocks.findOne({ block_id: "TSM0004" })         // 한 건 (없으면 null)

db.blocks.find({ category: "model" })
         .sort({ score: -1 })                      // -1 내림차순
         .skip(20).limit(10);                      // 페이지네이션

db.blocks.countDocuments({ category: "model" })
db.blocks.distinct("category")
```

**필터 JSON 안의 규칙 두 가지가 전부다.**

```javascript
{ 필드: 값 }                    // 같다
{ 필드: { $연산자: 값 } }        // 그 외 모든 조건
{ 필드1: ..., 필드2: ... }       // 최상위 나열 = AND
```

## 조건 연산자

```javascript
db.blocks.find({ score: { $gte: 0.5, $lt: 0.9 } })          // 0.5 <= score < 0.9
db.blocks.find({ category: { $in: ["model", "evaluate"] } })
db.blocks.find({ category: { $ne: "deprecated" } })
db.blocks.find({ alias: { $exists: false } })                // 필드 없음
db.blocks.find({ name: { $regex: "^TSM", $options: "i" } })  // 앞 고정만 인덱스 사용

db.blocks.find({ $or: [ { category: "model" }, { score: { $gte: 0.9 } } ] })
db.blocks.find({ $and: [ {...}, {...} ] })                   // 같은 필드에 조건 2개일 때만 필요
```

중첩·배열은 dot notation과 `$elemMatch` → [[MongoDB 배열 쿼리]].

```javascript
db.conv.find({ "summary.turns": { $gte: 10 } })
db.conv.find({ messages: { $elemMatch: { role: "user", score: { $gte: 9 } } } })
```

## 삽입

```javascript
db.blocks.insertOne({ block_id: "TSM0004", name: "선형 회귀", category: "model" })
db.blocks.insertMany([ {...}, {...} ])
```

`_id`를 안 주면 드라이버가 ObjectId를 만든다 ([[MongoDB 문서 모델과 BSON]]).

## 갱신

```javascript
// 일부 필드만
db.blocks.updateOne(
    { block_id: "TSM0004" },
    { $set: { name: "선형 회귀", updated_at: new Date() },
      $inc: { revision: 1 } }
)

// 여러 건
db.blocks.updateMany({ category: "model" }, { $set: { reviewed: true } })

// upsert
db.blocks.updateOne(
    { block_id: "TSM0004" },
    { $set: { name: "선형 회귀" }, $setOnInsert: { created_at: new Date() } },
    { upsert: true }
)

// 문서 전체 교체
db.blocks.replaceOne({ block_id: "TSM0004" }, { block_id: "TSM0004", name: "..." }, { upsert: true })
```

> **`$`가 없으면 전체 교체로 해석된다.** `updateOne(filter, {name: "x"})`는 에러이거나(최신) 문서를 통째로 날린다. 갱신 연산자 전체는 [[MongoDB 갱신 연산자]].

## 삭제

```javascript
db.blocks.deleteOne({ block_id: "TSM0004" })
db.blocks.deleteMany({ deprecated: true })
db.blocks.deleteMany({})           // 전부 — 필터 빠뜨림 사고 1순위
```

## 집계

```javascript
db.blocks.aggregate([
    { $match:  { deprecated: false } },
    { $group:  { _id: "$category", n: { $sum: 1 }, avg: { $avg: "$score" } } },
    { $sort:   { n: -1 } },
    { $limit:  10 }
])
```

stage 목록은 [[MongoDB Aggregation Pipeline]], 안에서 쓰는 표현식은 [[MongoDB 집계 연산자]].

## 인덱스

```javascript
db.blocks.createIndex({ block_id: 1 }, { unique: true })
db.blocks.createIndex({ category: 1, score: -1 })
db.agent_trace.createIndex({ ts_start: 1 }, { expireAfterSeconds: 2592000 })
db.blocks.getIndexes()
db.blocks.dropIndex("category_1_score_-1")
```

## SQL 감각으로 외우는 대응

| 하려는 것 | 셸 문법 |
|---|---|
| `SELECT *` | `find({})` |
| `WHERE` | `find({조건})` |
| `SELECT 열` | 두 번째 인자 projection |
| `ORDER BY` | `.sort({필드: 1\|-1})` |
| `LIMIT/OFFSET` | `.limit(n).skip(m)` |
| `GROUP BY` | `aggregate([{$group: ...}])` |
| `INSERT` | `insertOne/insertMany` |
| `UPDATE ... SET` | `updateOne(filter, {$set: ...})` |
| `DELETE` | `deleteOne/deleteMany` |
| `CREATE INDEX` | `createIndex` |

전체 대조는 [[SQL ↔ MongoDB 문법 대조표]].

## 한 줄 정리

`db.컬렉션.메서드(필터, 옵션)` 한 줄이 문법의 전부이고, **필터는 `{필드: 값}` 또는 `{필드: {$연산자: 값}}`** 두 형태뿐이다.

## 관련

- [[MongoDB 쿼리 연산자]]
- [[MongoDB 갱신 연산자]]
- [[MongoDB 배열 쿼리]]
- [[MongoDB 집계 연산자]]
- [[MongoDB CRUD 메서드]]
- [[mongosh 실전]]
- [[SQL ↔ MongoDB 문법 대조표]]
- [[MongoDB MOC]]
