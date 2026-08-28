---
title: MongoDB
created: 2026-08-28
tags:
  - database
  - mongodb
---

# MongoDB

- MongoDB는 표가 아니라 **문서(document)** 를 저장하는 DB다. 문서 하나가 JSON 한 덩어리다.
- 프로젝트에서는 **AI Agent 원장**을 담당한다. Catalog projection, 대화, mutation, trace, 감사 기록.

## 용어 대응

```text
SQL          Mongo
표 table  →  컬렉션 collection
행 row    →  문서 document
열 column →  필드 field
```

## 기본 조회

```javascript
db.blocks.find({ block_id: "TSM0004" })      // 필터
db.blocks.findOne({ block_id: "TSM0004" })   // 첫 한 건
db.blocks.find({}).sort({ block_id: 1 })     // 1=오름차순, -1=내림차순
```

## 가장 큰 차이 — 스키마를 강제하지 않는다

같은 컬렉션 안에서 문서마다 필드가 달라도 DB는 막지 않는다. 그래서 실무에서는 **코드에서 스키마를 선언**하고 부팅 시 컬렉션과 [[인덱스(Index)|인덱스]]를 만든다.

```python
SCHEMA = MongoCollectionSchema(
    name="workflow_conversations",
    fields=(MongoFieldSpec("session_id", "str", True, "채팅 세션 ID"), ...),
    indexes=(MongoIndexSpec("session_id", {"unique": True}), ...),
)
```

DB가 안 지켜 주니 **계약을 코드가 소유**하는 구조다. 이게 문서 DB를 쓸 때 가장 먼저 정해야 할 규율이다.

## 한 줄 정리

MongoDB는 **JSON 문서를 그대로 저장하고, 스키마 강제를 애플리케이션에 맡기는 DB**다.

## 어디로 가면 되나

| 알고 싶은 것 | 노트 |
|---|---|
| 문서 안의 타입, `_id`, 16MB | [[MongoDB 문서 모델과 BSON]] |
| 조회 조건 문법 | [[MongoDB 쿼리 연산자]] · [[MongoDB 배열 쿼리]] |
| 수정 문법 | [[MongoDB 갱신 연산자]] · [[upsert]] |
| 실제 호출 메서드 | [[MongoDB CRUD 메서드]] · [[MongoDB 커서와 페이지네이션]] |
| 집계 | [[MongoDB Aggregation Pipeline]] · [[MongoDB 집계 연산자]] |
| 인덱스 (TTL·partial) | [[MongoDB 인덱스 실전]] |
| 설계 판단 | [[MongoDB 데이터 모델링]] |
| 장애·예외 | [[MongoDB 예외 처리]] |
| 셸에서 확인 | [[mongosh 실전]] |

전체 지도는 [[MongoDB MOC]].

## 관련

- [[RDBMS vs Document DB]]
- [[MongoDB 쿼리 연산자]]
- [[upsert]]
- [[MongoDB Aggregation Pipeline]]
- [[motor]]
- [[유니크 인덱스]]
- [[MongoDB MOC]]
- [[MongoDB 문서 모델과 BSON]]
- [[MongoDB 데이터 모델링]]
