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

> [!info] 폴더 경계
> **저장과 조회**(Qdrant·HNSW·MongoDB·인덱스)는 `Database/` 폴더가 소유한다 — [[데이터베이스 MOC]].
> 여기는 **무엇을 근거로 고르는가**를 다룬다. 아래 목록은 두 폴더를 가로지르는 주제 지도다.

## Foundations — 근거 만들기

- [[RAG(Retrieval-Augmented Generation)]]
- [[Retrieve-Generate 패턴]]
- [[AI-Ready Data]]
- [[청킹(Chunking)]]
- [[임베딩(Embedding)]]
- [[BGE-M3]]
- [[코사인 유사도]]
- [[임베딩 모델 로딩 패턴]]

## 어휘 검색 (인메모리 · DB 아님)

- [[BM25]]
- [[TF-IDF]]
- [[한국어 토크나이징과 BM25]]

## 검색 정책

- [[score_threshold]] — "근거 없음"을 표현하는 컷

## 벡터 저장소 → `Database/Vector/`

원리와 사용법은 DB 폴더가 소유한다.

- 원리: [[벡터 데이터베이스]] · [[ANN(근사 최근접 이웃)]] · [[Small World 그래프]] · [[HNSW]] · [[HNSW 파라미터]] · [[Recall과 ef 튜닝]]
- 사용법: [[Qdrant]] · [[Qdrant Point와 Payload]] · [[Qdrant query_points]] · [[Qdrant 메타데이터 필터]] · [[Payload Index]] · [[Pre-filtering vs Post-filtering]] · [[Qdrant 메서드 사전]] · [[LangChain VectorStore 메서드]] · [[Qdrant 인덱스 재빌드 전략]]

## Retrieval

- [[Hybrid Search]]
- [[Hybrid RAG Search]]
- [[구조화 검색 채널]]
- [[Reciprocal Rank Fusion]]
- [[Reranking]]
- [[Query Transformation]]
- [[Retrieval Evaluation]]

## 저장소 패턴 → `Database/Patterns/`

- [[AI Agent 컬렉션 지도]] — Mongo·Qdrant 컬렉션 전체 목록
- [[Repository 패턴과 Port]] · [[Snapshot 캐시와 무효화]] · [[Degraded와 fail-soft 저장소]] · [[Seed 배포와 멱등 동기화]]

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
