---
title: BGE-M3
created: 2026-06-26
tags:
  - ai-agent
  - rag
  - embedding
  - model
---

# BGE-M3

BGE-M3는 BAAI 계열의 다국어 임베딩 모델이다. 한국어와 영어가 섞인 문서, 코드명, 설명을 벡터로 바꿔 [[벡터 데이터베이스]] 검색에 사용한다.

프로젝트에서는 기본 임베딩 모델로 `BAAI/bge-m3`를 사용하고, Qdrant 컬렉션 차원은 1024로 맞춘다.

## 왜 쓰나

- 한국어 질의와 영어/코드 섞인 카탈로그를 같이 검색해야 한다.
- 짧은 블록 설명과 긴 문서를 모두 임베딩해야 한다.
- 코사인 유사도 기반 vector search에 적합하다.

## normalize_embeddings

임베딩 벡터를 정규화하면 벡터 길이가 1이 된다.

```python
encode_kwargs={
    "normalize_embeddings": True
}
```

이렇게 하면 내적과 [[코사인 유사도]]가 사실상 같은 의미가 되어 검색 점수 해석이 안정된다.

## BM25와의 관계

BGE-M3는 의미 유사도를 잘 잡지만, 정확한 블록 ID나 옵션명은 [[BM25]]가 더 잘 잡을 때가 많다.

그래서 프로젝트에서는 BGE-M3 vector search를 단독으로 쓰지 않고 [[Hybrid RAG Search]]의 한 채널로 사용한다.

## 한 줄 정리

BGE-M3는 **한국어 포함 다국어 텍스트를 1024차원 벡터로 바꿔 의미 검색을 가능하게 하는 임베딩 모델**이다.

## 관련

- [[임베딩(Embedding)]]
- [[Qdrant]]
- [[BM25]]
- [[Hybrid RAG Search]]
