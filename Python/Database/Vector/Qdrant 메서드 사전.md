---
title: Qdrant 메서드 사전
created: 2026-08-28
tags:
  - ai-agent
  - rag
  - vector-database
  - reference
---

# Qdrant 메서드 사전

`QdrantClient`에서 실제로 쓰는 메서드. [[MongoDB CRUD 메서드]]의 벡터 버전이다.

## 연결과 컬렉션

```python
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams

client = QdrantClient(url="http://localhost:6333")        # 운영은 https + api_key

existing = {c.name for c in client.get_collections().collections}
if collection_name not in existing:
    client.create_collection(
        collection_name=collection_name,
        vectors_config=VectorParams(size=1024, distance=Distance.COSINE),
    )
```

| 메서드 | 하는 일 |
|---|---|
| `get_collections()` | 컬렉션 목록 |
| `collection_exists(name)` | 존재 확인 |
| `create_collection(name, vectors_config=...)` | 차원·거리 고정해 생성 |
| `delete_collection(name)` | 통째 삭제 |
| `get_collection(name)` | 설정·상태·point 수 |
| `count(collection_name=...)` | point 개수 |

`create_collection`은 **멱등이 아니다.** 이미 있으면 예외다. 그래서 `get_collections()`로 먼저 확인하는 idempotent 래퍼를 둔다.

## 쓰기

```python
client.upsert(collection_name=name, points=[
    models.PointStruct(id=point_id, vector=vec, payload={"block_id": "TSM0004"}),
])
client.delete(collection_name=name, points_selector=models.PointIdsList(points=stale_ids))
```

- `upsert`는 같은 id면 덮어쓴다 → [[upsert]]와 같은 성질.
- 삭제는 **id 목록**으로도, **필터**로도 가능하다.

```python
from qdrant_client.models import FieldCondition, Filter, MatchValue

client.delete(
    collection_name=name,
    points_selector=models.FilterSelector(
        filter=Filter(must=[FieldCondition(key="doc_id", match=MatchValue(value=doc_id))])
    ),
)
```

우리 코드에서 `Filter`가 등장하는 곳은 **이 삭제 경로뿐**이다. 검색에는 안 쓴다 ([[Pre-filtering vs Post-filtering]]).

## 읽기

```python
# 유사도 검색
res = client.query_points(collection_name=name, query=vec, limit=10, with_payload=True)

# id로 직접 조회 (유사도 아님)
points = client.retrieve(collection_name=name, ids=[pid1, pid2], with_payload=True)

# 전체 순회 — 페이지네이션
points, next_offset = client.scroll(collection_name=name, limit=256, with_vectors=False)
```

| 메서드 | 언제 |
|---|---|
| `query_points` | 의미 검색 ([[Qdrant query_points]]) |
| `retrieve` | id를 이미 아는 경우 |
| `scroll` | 전체 point id 수집 — 재빌드 시 stale 판별 |
| `create_payload_index` | 필터 쓸 필드에 ([[Payload Index]]) |

## 실제 사용 예 — 재빌드 중 검증

```python
def _get_collection_count(store) -> int:
    try:
        return store._client.count(collection_name=store.collection_name).count
    except Exception as exc:
        logger.warning("컬렉션 count 조회 실패: %s", exc)
        return 0
```

`scroll`로 현재 id 전체를 모으고, upsert 후 다시 모아 **기대 집합과 비교**한다. 자세히는 [[Qdrant 인덱스 재빌드 전략]].

## HTTP로 직접 확인

```bash
curl -s localhost:6333/collections | jq
curl -s localhost:6333/collections/block-catalog-bge-m3 | jq '.result.config.params'
curl -s -X POST localhost:6333/collections/block-catalog-bge-m3/points/count \
  -H 'content-type: application/json' -d '{"exact":true}' | jq
```

SDK 없이도 차원·거리·[[HNSW 파라미터]]·point 수를 바로 볼 수 있다. 디버깅의 첫 수단이다.

## 한 줄 정리

`create_collection`으로 공간을 정하고, `upsert`/`delete`로 세대를 관리하고, `query_points`/`scroll`로 읽는다.

## 관련

- [[Qdrant]]
- [[Qdrant query_points]]
- [[Qdrant Point와 Payload]]
- [[Qdrant 인덱스 재빌드 전략]]
- [[Payload Index]]
- [[LangChain VectorStore 메서드]]
- [[MongoDB CRUD 메서드]]
