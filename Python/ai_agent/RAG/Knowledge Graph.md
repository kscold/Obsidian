- Knowledge Graph(KG) = 세상을 **엔티티(노드)와 관계(엣지)** 의 그래프로 표현한 지식 베이스. RDF/OWL 같은 시맨틱 웹 표준이 뿌리.
- 일반 [[RAG(Retrieval-Augmented Generation)]]가 비슷한 문서를 찾는 데 그친다면, KG는 **다단계 추론**과 **수치·관계의 정확성**에서 강력하다.

## 그래프 표현

```
(타이거나스닥100 ETF) -[추종]-> (나스닥100 지수)
(나스닥100 지수)       -[포함]-> (애플)
(애플)                 -[속한산업]-> (IT)
```

- "성장주 ETF 추천해줘" → ETF → 추종 지수 → 구성 종목 → 산업 분류까지 한 번에 탐색.

## 대표 DB

| DB | 특징 |
|----|------|
| Neo4j | 가장 대중적. Cypher 쿼리. 매니지드 Aura |
| Amazon Neptune | AWS 매니지드, RDF + Property Graph |
| ArangoDB | Multi-model (Graph + Doc + KV) |
| TigerGraph | 대규모 분산 |
| Memgraph | 메모리 기반, 빠름 |

## Cypher 예 (Neo4j)

```cypher
// 노드/관계 적재
CREATE (a:Company {name: "애플"})
CREATE (b:ETF {name: "타이거나스닥100"})
CREATE (b)-[:HOLDS {weight: 0.07}]->(a)

// 다단계 탐색
MATCH (e:ETF)-[:TRACKS]->(idx)-[:CONTAINS]->(c:Company)
WHERE c.sector = "IT"
RETURN e.name, c.name
```

## LLM과 결합 — Text-to-Cypher

```python
prompt = f"""스키마:
- (ETF)-[:TRACKS]->(Index)-[:CONTAINS]->(Company)
- Company.sector

질문: "IT 비중이 높은 ETF는?"

Cypher 쿼리만 출력해라."""

cypher = llm.invoke(prompt)
result = neo4j.run(cypher).data()
answer = llm.invoke(f"질문: ...\n결과: {result}\n답해라")
```

- LLM의 일은 **자연어 → Cypher** 변환과 **결과 → 자연어** 정리. 사실은 DB가 책임진다.

## [[GraphRAG]]와의 관계

- GraphRAG는 KG를 **문서에서 자동 추출**해 RAG 검색에 활용하는 패턴.
- 잘 만들어진 도메인 KG가 이미 있다면 GraphRAG보다 직접 Text-to-Cypher가 더 정확하다.
- 둘을 섞기도 함: 도메인 KG는 사람이 만든 것 + 문서에서 자동 추출한 보조 KG.

## [[Ontology]]와의 관계

- **Ontology** = KG의 **스키마** (어떤 엔티티 타입과 관계가 존재하는가).
- KG = Ontology에 데이터를 채운 인스턴스.
- 미래에셋 사례: 금융 도메인 Ontology(상품·발행사·종목·산업) → 실제 ETF 데이터로 KG 구축.

## 장점

- **다단계 추론** — JOIN 여러 번이 필요한 질문에 강함.
- **수치 정확** — RAG가 약한 영역.
- **설명 가능** — 답의 경로를 그래프로 보여줄 수 있다.

## 단점

- 구축 비용 — 도메인 모델링이 사람의 일.
- 변화에 약함 — 새 관계 타입을 추가하면 쿼리·LLM 프롬프트 모두 갱신.
- LLM이 Cypher를 틀리면 답이 깨진다 → 스키마 description, few-shot 예시 필수.

## 관련

- [[Ontology]] · [[GraphRAG]] · [[RAG(Retrieval-Augmented Generation)]] · [[Hybrid Search]].
- [[Tool Calling]] — `query_graph_db` 도구로 노출.
