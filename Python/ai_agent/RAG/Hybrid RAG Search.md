---
title: Hybrid RAG Search
created: 2026-06-26
tags:
  - ai-agent
  - rag
  - search
---

# Hybrid RAG Search

Hybrid RAG Search는 여러 검색 채널을 동시에 사용해 더 안정적인 후보를 찾는 방식이다.

프로젝트의 블록 카탈로그 검색은 vector, [[BM25]], graph, document filter 네 채널을 돌린 뒤 [[Reciprocal Rank Fusion]]으로 합친다.

## 4채널

| 채널 | 역할 |
|---|---|
| Vector | [[BGE-M3]] 임베딩으로 의미 유사 검색 |
| BM25 | 블록 ID, 옵션명, 키워드 정확 매칭 |
| Graph | `requires_before`, `often_paired` 같은 관계 확장 |
| Document filter | 카테고리, 메타데이터 기반 구조화 필터 |

## 왜 단일 검색이 부족한가

- Vector만 쓰면 `BLOCK_042` 같은 정확 ID 검색이 약할 수 있다.
- BM25만 쓰면 "분류 모델 전에 범주형 변환" 같은 의미를 놓칠 수 있다.
- Graph가 없으면 필수 선행 블록을 못 끌고 올 수 있다.
- Document filter가 없으면 시각화/모델링 같은 큰 범주가 흔들릴 수 있다.

## fallback

검색 결과가 부족하면 단계적으로 완화한다.

1. hybrid 검색
2. pure vector 검색
3. threshold 완화
4. core block set fallback

이 구조는 [[Fallback]]의 검색 버전이다.

## 한 줄 정리

Hybrid RAG Search는 **의미 검색, 키워드 검색, 그래프 관계, 구조화 필터를 합쳐 RAG 후보의 안정성을 높이는 검색 전략**이다.

## 관련

- [[BM25]]
- [[BGE-M3]]
- [[Qdrant]]
- [[Reciprocal Rank Fusion]]
- [[GraphRAG]]
