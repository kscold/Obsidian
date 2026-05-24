- Ontology = **도메인의 개념(클래스)·속성·관계를 형식적으로 정의한 스키마**. 한 줄로 "지식에 대한 지도".
- [[Knowledge Graph]]가 실제 데이터의 인스턴스라면, Ontology는 그 데이터가 따라야 할 **타입 시스템**이다. AI-Ready Data의 핵심 자산.

## 일상 비유

| 영역 | Ontology에 해당 | KG에 해당 |
|------|------------------|------------|
| 도서관 | 듀이십진분류표 | 실제 책 |
| 회사 | 조직도·직무 체계 | 실제 직원 |
| RDB | DDL(테이블/컬럼 정의) | 행(row) 데이터 |

## 표현 방식

- **RDF/OWL** — W3C 표준. 가장 형식적이지만 무거움.
- **JSON Schema / YAML** — 가볍게, 도메인별로.
- **그냥 markdown 문서** — 작은 팀은 이정도면 충분.
- **Property Graph 스키마** — Neo4j의 노드/관계 라벨과 속성 타입.

## 작은 예 (YAML)

```yaml
entities:
  ETF:
    properties:
      isin: string
      name: string
      issuer: ref(Issuer)
      underlying_index: ref(Index)
  Index:
    properties:
      name: string
      constituents: list[ref(Company)]
  Company:
    properties:
      name: string
      ticker: string
      sector: enum["IT","Finance","Healthcare",...]

relations:
  TRACKS:     {from: ETF,   to: Index}
  CONTAINS:   {from: Index, to: Company}
  ISSUED_BY:  {from: ETF,   to: Issuer}
```

## Ontology가 LLM 활용에 주는 효과

1. **Text-to-Cypher / Text-to-SQL 정확도 ↑** — LLM에 스키마를 명확히 주면 쿼리가 정확해진다.
2. **데이터 추출 일관성 ↑** — 문서에서 엔티티를 뽑을 때 타입과 관계가 정의돼 있어 노이즈가 줄어든다.
3. **결과 설명 가능** — 답이 어느 엔티티/관계를 거쳐 도출됐는지 추적 가능.

## 도메인 모델링 절차

1. **사람이 풀고 싶은 질문**을 먼저 적는다 (예: "성장주 ETF에서 IT 비중이 가장 높은 것?").
2. 그 질문에 필요한 **명사**를 엔티티 후보로 (ETF, 산업, 회사).
3. **동사**를 관계 후보로 (추종, 포함, 소속).
4. 속성·기수성(1:N, M:N)·필수/선택 정리.
5. 소수 케이스로 검증 — 정의된 스키마로 실제 질문이 풀리는지.

## 미래에셋 사례 요약

- 금융상품(ETF/공모펀드/국내 채권) 도메인의 Ontology를 먼저 정의.
- 그 위에 [[Knowledge Graph|KG]]를 구축하고 [[GraphRAG]]·[[Hybrid Search]]를 결합.
- 결과: 일반 RAG로 풀 수 없던 다단계 질의(상품 → 지수 → 종목 → 산업) 정확도 비약 상승.

## 한계

- 구축이 사람 일이고 변화에 보수적 — 너무 상세히 잡으면 유지 불가.
- 비정형 텍스트만 다루는 도메인에는 과한 인프라.
- Ontology가 잘못되면 그 위의 모든 KG·쿼리가 흔들림.

## 관련

- [[Knowledge Graph]] · [[GraphRAG]] · [[AI-Ready Data]].
- [[Tool Calling]] — Ontology를 LLM 프롬프트에 주입해 쿼리 정확도 ↑.
