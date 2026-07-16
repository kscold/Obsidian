---
title: Graph Traversal Retrieval
created: 2026-07-15
tags:
  - ai-agent
  - rag
  - graph-database
  - neo4j
---

# Graph Traversal Retrieval

Graph Traversal Retrieval은 벡터·키워드 검색이 찾은 후보를 시작점으로 삼아, 의미가 명시된 그래프 관계를 제한적으로 따라가며 추가 근거를 찾는 검색 기법이다. [[GraphRAG]]처럼 문서 전체에서 그래프를 새로 추출하는 방식과 달리, 기존 도메인 그래프를 **retrieval 채널 하나**로 쓰는 데 초점이 있다.

## 기본 흐름

~~~mermaid
flowchart LR
    Query["질문"] --> Seed["Vector / BM25 후보"]
    Seed --> Traverse["관계 allowlist의 제한된 hop 탐색"]
    Traverse --> Fuse["원 검색 결과와 융합"]
    Fuse --> Rerank["필요 시 rerank"]
    Rerank --> Context["근거와 경로를 포함한 컨텍스트"]
~~~

그래프 탐색은 처음부터 전체 그래프를 뒤지는 것이 아니다. semantic 또는 lexical 검색으로 얻은 **작은 seed 집합**에서 시작해, 질문과 관련된 관계만 정해진 거리까지 확장한다.

## 탐색 정책

| 정책 | 이유 |
|---|---|
| 관계 allowlist | DEPENDS_ON, REQUIRES, RELATED_TO처럼 의미가 검증된 관계만 따른다. |
| 최대 hop | 보통 1~2 hop으로 제한해 고차수 노드 폭발을 막는다. |
| 후보 상한 | seed 수, 경로 수, 반환 노드 수에 각각 limit을 둔다. |
| 관계 가중치 | 필수 의존성, 약한 연관성, 공동 사용을 같은 점수로 보지 않는다. |
| provenance | 후보가 어떤 seed와 어떤 경로로 발견됐는지 함께 보존한다. |

~~~cypher
MATCH (seed:Entity {id: $seed_id})
MATCH path = (seed)-[:DEPENDS_ON|RELATED_TO*1..2]-(candidate:Entity)
WHERE candidate.id <> $seed_id
RETURN candidate.id, min(length(path)) AS hops
ORDER BY hops
LIMIT $limit
~~~

노드 ID와 limit은 parameter로 전달한다. 관계 타입과 hop 수는 정책 코드에서 고정하거나 allowlist로 선택한다. 사용자 문장을 Cypher 문자열에 이어 붙여서는 안 된다.

## 점수와 융합

그래프 채널은 기존 vector 또는 BM25 점수를 덮어쓰지 않는다. 별도 ranked list로 만든 뒤 [[Reciprocal Rank Fusion]]이나 명시적인 가중치로 융합한다.

~~~text
final_score =
  vector_score
  + lexical_score
  + graph_relation_weight
  - hop_penalty
~~~

같은 seed를 다시 높은 점수로 넣어 이중 계산하지 않도록, graph 채널은 새로 발견한 이웃만 기여하게 하거나 채널별 기여도를 별도로 기록하는 방법이 있다.

## 언제 유용한가

- 문서·기능·상품·정책 사이의 의존성, 전제 조건, 대체 관계가 실제로 관리될 때
- 한 청크만으로 답이 완성되지 않고 관련 항목을 함께 찾아야 할 때
- 그래프 관계의 출처와 품질을 운영할 수 있을 때

관계가 임의 추출된 노이즈라면 그래프 확장은 recall보다 오염을 더 크게 만들 수 있다. 그 경우에는 [[Hybrid Search]], [[Reranking]], metadata filter부터 안정화하는 편이 낫다.

## 평가

전체 RAG 점수만 보지 말고 graph 채널을 껐을 때와 비교한다.

1. vector만 사용
2. vector + BM25
3. vector + BM25 + graph traversal
4. 전체 + reranker

각 단계의 Recall@k, Precision@k, latency, 경로 길이, 빈 결과 비율을 측정한다. graph가 정답 근거를 추가로 가져온 실제 사례와, 불필요한 이웃을 늘린 사례를 같이 저장해야 관계 정책을 개선할 수 있다.

## 관련

- [[Graph Data Modeling]]
- [[Knowledge Graph]]
- [[Hybrid RAG Search]]
- [[Reciprocal Rank Fusion]]
- [[Retrieval Evaluation]]
- [[GraphRAG]]

## 공식 문서

- [Neo4j Cypher 질의](https://neo4j.com/docs/cypher-manual/current/queries/)
- [Neo4j 인덱스](https://neo4j.com/docs/cypher-manual/current/indexes/)
