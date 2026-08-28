---
title: Qdrant Point와 Payload
created: 2026-08-28
tags:
  - ai-agent
  - rag
  - vector-database
---

# Qdrant Point와 Payload

[[Qdrant]]의 저장 단위는 **point** 하나다.

```text
Collection (예: block-catalog-bge-m3)
└─ Point
   ├─ id       결정적 UUID          ← 같은 내용이면 항상 같은 ID
   ├─ vector   [0.02, -0.04, ...]   1024개 ([[BGE-M3]])
   └─ payload  { block_id, source, category, ... }   ← 메타데이터 JSON
```

| 구성 | [[MongoDB]]로 치면 | 하는 일 |
|---|---|---|
| id | `_id` | 갱신·삭제의 기준 |
| vector | (없음) | 유사도 계산 대상 |
| payload | 나머지 필드 전부 | 필터 조건 + 결과로 돌려받을 내용 |

## payload가 중요한 이유

벡터만으로는 "이 사용자의 문서만", "deprecated 아닌 것만" 같은 조건을 표현할 수 없다. 유사도에는 그런 개념이 없다. **그 자리를 payload가 채운다.**

- 검색 조건으로: [[Qdrant 메타데이터 필터]]
- 결과로 받기: `with_payload=True`
- 필터를 자주 걸 필드는 [[Payload Index]]가 필요하다

## 컬렉션은 벡터 공간이다

[[MongoDB]] 컬렉션은 문서 묶음일 뿐이지만, Qdrant 컬렉션은 **차원과 거리 함수가 고정된 공간**이다.

```python
client.create_collection(
    collection_name="block-catalog-bge-m3",
    vectors_config=VectorParams(size=1024, distance=Distance.COSINE),
)
```

생성 후 차원·거리를 못 바꾼다. [[임베딩(Embedding)|임베딩]] 모델을 바꾸면 컬렉션을 새로 만들어야 하고, 그래서 컬렉션 이름에 모델명(`-bge-m3`)을 박아 둔다.

## point ID를 결정적으로 만드는 이유

내용 해시로 ID를 만들면 **같은 문서는 항상 같은 point**가 된다. 재실행이 [[upsert|멱등]]해지고, 새 세대에 없는 ID만 골라 지울 수 있다. 자세히는 [[Qdrant 인덱스 재빌드 전략]].

## 한 줄 정리

point = **id + vector + payload**이고, 벡터가 못 하는 조건 표현을 payload가 담당한다.

## 관련

- [[Qdrant]]
- [[Qdrant query_points]]
- [[Qdrant 메타데이터 필터]]
- [[Payload Index]]
- [[Qdrant 인덱스 재빌드 전략]]
- [[projection]]
