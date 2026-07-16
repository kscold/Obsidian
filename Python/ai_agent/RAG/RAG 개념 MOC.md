---
title: RAG 개념 MOC
created: 2026-07-15
tags:
  - ai-agent
  - rag
  - moc
---

# RAG 개념 MOC

RAG는 검색 결과를 LLM 컨텍스트로 넣는 기능만이 아니라, **근거를 찾고, 순위를 매기고, 검증 가능한 답변으로 연결하는 전체 시스템**이다.

## Foundations

- [[RAG(Retrieval-Augmented Generation)]]
- [[Retrieve-Generate 패턴]]
- [[청킹(Chunking)]]
- [[임베딩(Embedding)]]
- [[코사인 유사도]]
- [[벡터 데이터베이스]]
- [[BM25]]
- [[BGE-M3]]
- [[Qdrant]]

## Retrieval

- [[Hybrid Search]]
- [[Hybrid RAG Search]]
- [[Reciprocal Rank Fusion]]
- [[Reranking]]
- [[Query Transformation]]
- [[Retrieval Evaluation]]

## Graph

- [[Knowledge Graph]]
- [[Ontology]]
- [[Graph Data Modeling]]
- [[Graph Traversal Retrieval]]
- [[GraphRAG]]
- [[Lexical Graph RAG]]

## Agentic

- [[Agentic RAG]]
- [[RAG Agent]]
- [[Agentic Search Tool]]

## Quality

- [[Grounded Generation]]
- [[Evaluation]]
- [[LLM-as-Judge]]

## 설계 순서

1. [[AI-Ready Data|근거 데이터]]를 정리하고 문서 단위를 정한다.
2. 기본 검색의 Recall을 [[Retrieval Evaluation|측정]]한다.
3. 필요할 때만 hybrid, reranking, graph 확장, query transformation을 추가한다.
4. 최종 답변이 근거를 실제로 따르는지는 [[Grounded Generation|별도로 검증]]한다.
