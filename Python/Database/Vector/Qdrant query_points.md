---
title: Qdrant query_points
created: 2026-08-28
tags:
  - ai-agent
  - rag
  - vector-database
  - query
---

# Qdrant query_points

- `query_points()`는 [[Qdrant]] SDK(v1.10+)의 **통합 검색 진입점**이다. 구버전 `search()`의 후계이고, 벡터 검색·필터·추천·하이브리드를 한 인터페이스로 받는다.

```python
response = client.query_points(
    collection_name="block-catalog-bge-m3",
    query=query_vector,        # 1024차원 float 리스트
    limit=10,
    with_payload=True,
    with_vectors=False,
    score_threshold=0.3,
    query_filter=my_filter,
    search_params=models.SearchParams(hnsw_ef=128, exact=False),
)
for point in response.points:
    print(point.id, point.score, point.payload)
```

## 인자별로 무엇을 정하나

| 인자 | 정하는 것 | 관련 |
|---|---|---|
| `query` | 질의 벡터 (또는 point id) | [[임베딩(Embedding)]] |
| `limit` | 상위 K개 | — |
| `with_payload` | 메타데이터를 같이 받을지 | [[Qdrant Point와 Payload]] |
| `with_vectors` | 1024차원 원본까지 받을지 — 보통 `False` | [[projection]] |
| `score_threshold` | 점수 컷 | [[score_threshold]] |
| `query_filter` | payload 조건 | [[Qdrant 메타데이터 필터]] |
| `search_params` | `hnsw_ef`, `exact` | [[HNSW 파라미터]] |

## 우리 프로젝트는 이걸 직접 안 부른다

LangChain [[LangChain|래퍼]]를 거친다.

```python
store = QdrantVectorStore(client=client, collection_name=..., embedding=embedding)
results = store.similarity_search_with_score(user_prompt, k=15)   # (Document, score) 목록
```

| 차이 | 직접 호출 | LangChain 래퍼 |
|---|---|---|
| 임베딩 | 내가 벡터를 만들어 넣음 | 문자열을 주면 알아서 임베딩 |
| 결과 | `point.payload` | `document.metadata`, `document.page_content` |
| 필터·튜닝 | 전부 노출 | 일부만 노출 — 세밀한 제어는 client로 내려가야 함 |

편한 대신 `query_filter`나 `search_params` 같은 손잡이가 가려진다. 그래서 규모가 커지면 래퍼를 벗고 내려가게 된다.

## 한 줄 정리

`query_points`는 **벡터 + 개수 + 필터 + 탐색 다이얼을 한 번에 선언하는 검색 호출**이다.

## 관련

- [[Qdrant]]
- [[Qdrant Point와 Payload]]
- [[Qdrant 메타데이터 필터]]
- [[score_threshold]]
- [[HNSW 파라미터]]
- [[LangChain]]
