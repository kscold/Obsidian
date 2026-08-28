---
title: LangChain VectorStore 메서드
created: 2026-08-28
tags:
  - ai-agent
  - rag
  - vector-database
  - langchain
---

# LangChain VectorStore 메서드

- 우리 코드가 [[Qdrant]]를 만지는 **실제 경로**다. [[LangChain]]의 `QdrantVectorStore`가 클라이언트를 감싼다.
- 문자열을 주면 [[임베딩(Embedding)|임베딩]]까지 알아서 한다는 게 raw SDK와의 차이다.

## 팩토리

```python
def _ensure_collection(client: QdrantClient, collection_name: str) -> None:
    """컬렉션 없으면 cosine/1024dim 으로 생성 (idempotent)."""
    existing = {c.name for c in client.get_collections().collections}
    if collection_name not in existing:
        client.create_collection(
            collection_name=collection_name,
            vectors_config=VectorParams(size=CATALOG_EMBEDDING_DIM, distance=Distance.COSINE),
        )

def get_vectorstore(collection_name=RAG_VECTOR_COLLECTION_NAME, embedding=None):
    embedding = embedding or get_embeddings()
    client = get_qdrant_client()
    _ensure_collection(client, collection_name)
    return QdrantVectorStore(client=client, collection_name=collection_name, embedding=embedding)
```

[[Factory Pattern]] + [[Lazy Initialization]]. 연결이 무거우므로 import 시점이 아니라 실제 검색 때 만든다.

## 검색

```python
results = store.similarity_search_with_score(user_prompt, k=15)
for document, score in results:
    block_id = document.metadata.get("block_id")
    text = document.page_content
```

| 메서드 | 반환 | 비고 |
|---|---|---|
| `similarity_search(query, k)` | `list[Document]` | 점수를 안 준다 → [[score_threshold]] 못 씀 |
| `similarity_search_with_score(query, k)` | `list[(Document, float)]` | **우리가 쓰는 것.** cosine은 1에 가까울수록 좋음 |
| `similarity_search_by_vector(vec, k)` | 벡터를 직접 넣을 때 | 이미 임베딩이 있는 경우 |
| `max_marginal_relevance_search` | 다양성 고려 | 비슷한 문서 중복을 줄임 |
| `as_retriever(search_type, search_kwargs)` | Retriever 객체 | 체인에 꽂을 때 |

```python
return vs.as_retriever(search_type="similarity", search_kwargs={"k": 3})
```

## 쓰기 — id를 직접 준다

```python
written_ids = {str(pid) for pid in store.add_documents(batch, ids=batch_ids)}
if written_ids != set(batch_ids):
    raise RuntimeError(f"{label} Qdrant upsert 응답 ID가 요청과 다릅니다.")
```

`ids=`를 넘기면 [[Qdrant Point와 Payload|point id]]를 우리가 정한다. 내용 해시 기반 결정적 id를 쓰면 재실행이 [[upsert|멱등]]해진다. **반환된 id가 요청과 같은지 확인**하는 게 검증의 핵심이다 ([[Qdrant 인덱스 재빌드 전략]]).

## Document 구조

```python
Document(page_content="블록 설명 텍스트...", metadata={"block_id": "TSM0004", "source": "catalog"})
```

| LangChain | Qdrant |
|---|---|
| `page_content` | payload 안의 본문 필드 |
| `metadata` | payload 나머지 |
| (내부) | vector |

즉 `document.metadata`가 곧 payload다. 그래서 우리 필터링이 `document.metadata.get("block_id")`로 파이썬에서 이뤄진다.

## 래퍼의 대가

| | raw `QdrantClient` | `QdrantVectorStore` |
|---|---|---|
| 임베딩 | 직접 | 자동 |
| `query_filter` | 노출 | 가려짐 |
| `search_params`(hnsw_ef) | 노출 | 가려짐 |
| 결과 | `point.payload` | `document.metadata` |

편의를 얻고 **세밀한 제어를 잃는다.** [[Pre-filtering vs Post-filtering|서버 필터]]가 필요해지는 시점이 곧 래퍼를 벗는 시점이다.

## 한 줄 정리

VectorStore는 **임베딩 + Qdrant 호출을 묶은 래퍼**이고, `similarity_search_with_score`와 `add_documents(ids=)`가 우리가 쓰는 두 손잡이다.

## 관련

- [[Qdrant]]
- [[Qdrant 메서드 사전]]
- [[LangChain]]
- [[임베딩(Embedding)]]
- [[BGE-M3]]
- [[score_threshold]]
- [[Factory Pattern]]
- [[Lazy Initialization]]
