---
title: Qdrant
created: 2026-06-26
tags:
  - ai-agent
  - rag
  - vector-database
---

# Qdrant

Qdrant는 임베딩 벡터를 저장하고 유사도 검색을 수행하는 [[벡터 데이터베이스]]이다.

프로젝트에서는 [[BGE-M3]] 임베딩을 저장하고, 컬렉션마다 `cosine` 거리와 1024 차원을 사용한다.

## 컬렉션

컬렉션은 벡터를 담는 논리적 저장소이다.

예:

- `rag-bge-m3`
- `block-catalog-bge-m3`
- `pipeline-history-bge-m3`
- `error-patterns-bge-m3`

도메인별로 컬렉션을 나누면 채팅 RAG와 워크플로우 카탈로그 RAG가 서로 섞이지 않는다.

## VectorStore

LangChain에서는 Qdrant를 `QdrantVectorStore`로 감싸서 retriever처럼 사용한다.

```python
store = QdrantVectorStore(
    client=client,
    collection_name="block-catalog-bge-m3",
    embedding=embedding,
)
```

## lazy 연결

Qdrant 연결은 무겁기 때문에 모듈 import 시점이 아니라 실제 검색이 필요할 때 초기화하는 편이 안전하다. 이것이 [[Lazy Initialization]] 패턴이다.

## 한 줄 정리

Qdrant는 **임베딩 벡터를 저장하고 코사인 유사도로 비슷한 문서를 찾아주는 벡터 DB**이다.

## 관련

- [[BGE-M3]]
- [[벡터 데이터베이스]]
- [[코사인 유사도]]
- [[Hybrid RAG Search]]
