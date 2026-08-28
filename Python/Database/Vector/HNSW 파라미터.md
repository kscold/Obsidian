---
title: HNSW 파라미터
created: 2026-08-28
tags:
  - ai-agent
  - rag
  - vector-database
  - tuning
---

# HNSW 파라미터

[[HNSW]]에는 손댈 수 있는 다이얼이 셋 있다. **정하는 시점이 다르다는 게 핵심**이다.

| 파라미터 | 언제 정하나 | 뜻 | 올리면 |
|---|---|---|---|
| **M** | 컬렉션 생성 시 | 점 하나가 가질 이웃(간선) 수 | 정확도 ↑, 메모리 ↑ |
| **ef_construct** | 인덱스 빌드 시 | 그래프를 만들 때 후보를 얼마나 넓게 볼지 | 그래프 품질 ↑, 빌드 시간 ↑ |
| **hnsw_ef** (ef_search) | **검색할 때마다** | 탐색 중 유지하는 후보 목록의 폭 | [[Recall과 ef 튜닝\|recall]] ↑, 응답 느려짐 |

```mermaid
flowchart LR
    C["컬렉션 생성<br/>M 고정"] --> B["인덱스 빌드<br/>ef_construct"]
    B --> S["검색 (매 요청)<br/>hnsw_ef · exact"]
    S -.재조정 가능.-> S
```

**앞의 둘은 나중에 바꾸려면 재빌드가 필요하고, hnsw_ef만 요청마다 바꿀 수 있다.** 그래서 실무 튜닝은 대부분 hnsw_ef로 한다.

## M — 그래프의 밀도

- 작으면(8~16): 메모리 적고 빠르지만, 지름길이 부족해 [[Small World 그래프|greedy 탐색]]이 길을 잃기 쉽다.
- 크면(32~64): 정확하지만 점마다 이웃 목록이 커져 메모리가 늘고 삽입도 느려진다.
- 고차원일수록 더 큰 M이 필요하다. 기본값(보통 16)에서 시작해 recall이 부족할 때 올린다.

## exact — 다이얼을 끄는 스위치

```python
search_params = models.SearchParams(hnsw_ef=128, exact=False)
```

`exact=True`면 HNSW를 건너뛰고 전수 비교한다([[풀 스캔(Full Scan)]]). 운영에 쓰는 값이 아니라 **정답지를 만드는 용도**다. exact 결과와 ANN 결과를 비교해야 recall을 측정할 수 있다.

## 언제 손대야 하나

문서가 수만 건을 넘고, 검색 품질 문제가 [[임베딩(Embedding)|임베딩]]이나 쿼리 쪽이 아니라고 확인됐을 때다. 그전에 파라미터부터 만지면 원인을 못 찾는다.

## 한 줄 정리

M과 ef_construct는 **만들 때 정하는 구조**, hnsw_ef는 **요청마다 돌리는 다이얼**이다.

## 관련

- [[HNSW]]
- [[Recall과 ef 튜닝]]
- [[ANN(근사 최근접 이웃)]]
- [[Qdrant query_points]]
