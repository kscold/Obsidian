---
title: motor
created: 2026-08-28
tags:
  - database
  - mongodb
  - python
---

# motor

- motor는 **비동기 MongoDB 드라이버**다. pymongo의 async 버전이라고 보면 된다.
- [[async-await]] 기반 서버(FastAPI, gRPC async, LangGraph 노드)에서 DB를 기다리는 동안 이벤트 루프를 막지 않으려고 쓴다.

```python
from motor.motor_asyncio import AsyncIOMotorClient

client = AsyncIOMotorClient(uri, serverSelectionTimeoutMS=5000, connectTimeoutMS=5000)
db = client["ai_agent"]

doc = await db.blocks.find_one({"block_id": "TSM0004"})   # await 필요
async for row in db.blocks.find({}, {"_id": 0}):          # 커서는 async for
    ...
```

## 실무 포인트 셋

**1) 커넥션은 [[Singleton Pattern|싱글톤]]** — 클라이언트 하나가 커넥션 풀을 들고 있다. 요청마다 새로 만들면 풀이 폭발한다. 보통 [[Lazy Initialization|lazy]] 전역 하나로 잡는다.

**2) timeout을 짧게** — 기본 서버 선택 타임아웃이 30초다. DB가 죽었을 때 30초를 기다리면 그 위의 [[Fail-soft|fail-soft]]가 의미를 잃는다. 5초 정도로 줄여 빨리 실패시킨다.

**3) 인덱스는 부팅 때 한 번** — `create_index`는 멱등이라 매번 호출해도 되지만, 요청 경로가 아니라 시작 시점에 모아서 만든다.

```python
for schema in MONGO_COLLECTION_SCHEMAS:
    collection = db[schema.name]
    for index in schema.indexes:
        await collection.create_index(index.keys, **index.options)
```

## 한 줄 정리

motor는 **await로 쓰는 MongoDB 드라이버**이고, 싱글톤 + 짧은 timeout + 부팅 시 인덱스 생성이 기본 셋업이다.

## 관련

- [[MongoDB]]
- [[async-await]]
- [[Singleton Pattern]]
- [[Lazy Initialization]]
- [[인덱스(Index)]]
