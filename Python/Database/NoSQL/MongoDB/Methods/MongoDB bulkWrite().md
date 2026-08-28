---
title: MongoDB bulkWrite()
created: 2026-08-28
tags:
  - database
  - mongodb
  - method
  - performance
---

# MongoDB bulkWrite()

**서로 다른 쓰기 작업 여러 개를 한 번의 왕복으로 보낸다.**

`insertMany`는 삽입만, `updateMany`는 같은 값으로만 바꾼다. **문서마다 다른 내용**을 써야 하면 이게 답이다.

```javascript
db.<컬렉션>.bulkWrite( [ <작업>, <작업>, ... ], { ordered: true|false } )
```

## 작업 종류

| 작업 | 대응 메서드 |
|---|---|
| `InsertOne` | [[MongoDB insertOne() insertMany()\|insertOne]] |
| `UpdateOne` / `UpdateMany` | [[MongoDB updateOne()\|updateOne]] |
| `ReplaceOne` | [[MongoDB replaceOne()\|replaceOne]] |
| `DeleteOne` / `DeleteMany` | [[MongoDB deleteOne() deleteMany()\|deleteOne]] |

## 실제 코드

```python
from pymongo import UpdateOne

await db[AGENT_TRACE_COLLECTION].bulk_write(
    [
        UpdateOne(
            {"span_id": document["span_id"]},
            {"$setOnInsert": document},
            upsert=True,
        )
        for document in docs
    ],
    ordered=False,
)
```

trace span 수십 개를 **한 번에** 저장한다. 각 span이 자기 필터를 갖고, `$setOnInsert` + `upsert`라 **이미 있는 span은 건드리지 않는다**(재전송 안전).

## 왜 빠른가

```mermaid
flowchart TD
    subgraph L["개별 호출 100번"]
        A1["요청→응답"] --> A2["요청→응답"] --> A3["... 100회"]
        A3 --> T1["네트워크 왕복 100회"]
    end
    subgraph B["bulkWrite 1번"]
        B1["작업 100개를 한 배열로"] --> T2["네트워크 왕복 1회"]
    end
```

**병목은 대개 계산이 아니라 왕복(round trip)이다.** 수백 건이면 체감 차이가 크다.

## ordered

| 값 | 동작 |
|---|---|
| `True` (기본) | 순서대로, 실패하면 **거기서 중단** |
| `False` | 순서 무관, 실패한 것만 건너뛰고 **나머지 진행**, 병렬 처리 가능 |

관측 원장처럼 **일부 실패가 나머지를 막으면 안 되는** 경우 `ordered=False`다. 작업 간 순서 의존이 있으면 `True`.

## 결과

```python
res = await col.bulk_write(ops, ordered=False)
res.inserted_count, res.matched_count, res.modified_count
res.upserted_count, res.deleted_count
res.upserted_ids          # {작업 인덱스: _id}
```

## 부분 실패

```python
from pymongo.errors import BulkWriteError

try:
    await col.bulk_write(ops, ordered=False)
except BulkWriteError as e:
    e.details["nInserted"], e.details["writeErrors"]
```

`ordered=False`에서 예외가 나도 **일부는 이미 저장돼 있다.** 전부 아니면 전무가 필요하면 [[트랜잭션(ACID)|트랜잭션]]이 있어야 한다 — bulkWrite 자체는 원자적이지 않다(문서 단위로만 원자적).

## 주의

- 작업 배열은 **한 번에 10만 개**까지. 넘으면 드라이버가 쪼갠다.
- 요청 전체가 BSON 크기 제한을 받는다. 문서가 크면 배치를 작게 나눈다.
- 작업 목록을 만드는 것 자체가 메모리를 쓴다. 수십만 건은 청크로 끊는다.

## 한 줄 정리

**문서마다 다른 쓰기**를 한 번의 왕복으로 보내는 도구이고, `ordered=False`가 로그·원장 적재의 기본값이다.

## 관련

- [[MongoDB updateOne()]]
- [[MongoDB insertOne() insertMany()]]
- [[MongoDB 쓰기 결과 객체]]
- [[MongoDB 갱신 연산자]]
- [[MongoDB 예외 처리]]
- [[Trajectory]]
- [[MongoDB CRUD 메서드]]
