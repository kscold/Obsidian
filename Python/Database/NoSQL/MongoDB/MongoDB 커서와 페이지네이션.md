---
title: MongoDB 커서와 페이지네이션
created: 2026-08-28
tags:
  - database
  - mongodb
  - performance
---

# MongoDB 커서와 페이지네이션

- `find()`는 **결과를 주지 않는다.** 커서(cursor)를 준다. 커서는 "서버에 있는 결과 집합을 가리키는 손잡이"다.
- 그래서 `find()`에는 `await`가 없고, **소비할 때** 네트워크가 오간다.

```python
cursor = db.blocks.find({}, {"_id": 0})     # 아직 아무것도 안 일어남
async for doc in cursor:                    # 여기서 batch 단위로 받아온다
    ...
```

```mermaid
flowchart LR
    F["find() 호출"] --> C["커서 생성<br/>(쿼리 아직 안 감)"]
    C --> I["첫 소비"]
    I --> B1["batch 1 (기본 101건)"]
    B1 --> B2["batch 2 (~16MB)"]
    B2 --> B3["..."]
    B3 --> E["소진 · 커서 닫힘"]
```

## 세 가지 소비 방법

```python
# 1) async for — 스트리밍. 메모리 안전. 큰 결과에 기본
async for doc in db.blocks.find({}):
    ...

# 2) to_list(length=N) — N건까지 메모리에 적재
docs = await db.agent_prompts.find(query, {"_id": 0}).to_list(length=200)

# 3) find_one — 한 건이면 커서를 안 만든다
doc = await db.blocks.find_one({"block_id": "TSM0004"})
```

`to_list(length=None)`은 **전부 메모리에 올린다.** 컬렉션이 커지면 그 순간 서버가 죽는다. 우리 코드가 항상 `length=`를 명시하는 이유다.

## sort · skip · limit

```python
cursor = (
    db.catalog_change_proposals.find(query, {"_id": 0})
    .sort("created_at", -1)      # -1 내림차순, 1 오름차순
    .limit(limit)
)
docs = await cursor.to_list(length=limit)
```

체인 순서는 상관없다. 서버가 항상 **sort → skip → limit** 순으로 처리한다.

```python
.sort([("owner_scope", 1), ("created_at", -1)])    # 복합 정렬
```

**정렬에는 인덱스가 필요하다.** 없으면 메모리에서 정렬하는데, MongoDB는 여기에 **32MB 상한**을 두고 넘으면 쿼리를 거부한다(`Sort exceeded memory limit`). 정렬 필드는 [[복합 인덱스]]에 포함시킨다.

## skip 페이지네이션의 함정

```python
.skip(10000).limit(20)     # 10,020건을 읽고 10,000건을 버린다
```

`skip`은 **건너뛴 문서를 실제로 다 읽는다.** 뒤로 갈수록 선형으로 느려진다. 100페이지째가 1페이지째보다 100배 느리다.

## 커서 페이지네이션 (keyset)

```python
# 1페이지
docs = await col.find({}).sort("_id", 1).limit(20).to_list(20)
last_id = docs[-1]["_id"]

# 다음 페이지 — skip 없이 "마지막 값 다음부터"
docs = await col.find({"_id": {"$gt": last_id}}).sort("_id", 1).limit(20).to_list(20)
```

| | skip/limit | keyset |
|---|---|---|
| 깊은 페이지 성능 | 선형으로 악화 | **일정** |
| 임의 페이지 점프 | 가능 | 불가 (다음/이전만) |
| 중간에 삽입되면 | 항목이 밀려 중복·누락 | 안정적 |

무한 스크롤·로그 조회는 keyset이 정답이다. `_id`는 [[MongoDB 문서 모델과 BSON|생성순에 가깝게 정렬]]되므로 커서 키로 쓰기 좋다.

## `count_documents` 주의

```python
n = await db.blocks.count_documents({"block_id": {"$in": seed_ids}})   # 정확, 스캔함
n = await db.blocks.estimated_document_count()                         # 메타데이터, 즉시, 필터 불가
```

전체 개수 표시 때문에 매 페이지마다 `count_documents`를 부르면 그게 병목이 된다. 우리 시딩 검증처럼 **정확한 수가 판단 근거일 때만** 쓴다 ([[Seed 배포와 멱등 동기화]]).

## 한 줄 정리

`find()`는 커서를 주고, 소비할 때 batch로 흐른다. **`to_list`에는 항상 length를**, 깊은 페이지에는 **skip 대신 keyset**을 쓴다.

## 관련

- [[MongoDB CRUD 메서드]]
- [[MongoDB 쿼리 연산자]]
- [[projection]]
- [[복합 인덱스]]
- [[motor]]
- [[explain과 실행 계획]]
- [[MongoDB MOC]]
