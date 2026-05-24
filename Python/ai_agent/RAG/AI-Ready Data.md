- AI-Ready Data = "**AI가 데이터를 이해하려면, 데이터가 AI를 이해할 수 있도록 준비돼야 한다**" 는 원칙. 단순히 데이터를 모은 것이 아니라 **의미·관계·품질이 정돈된 상태**.
- MIT 조사: GenAI 파일럿의 **95%가 production으로 못 간다**. 원인은 LLM 성능이 아니라 데이터 준비 부족.

## 일반 데이터 vs AI-Ready Data

| 일반 데이터 | AI-Ready Data |
|-------------|----------------|
| RDB 사일로, 비정형 문서 더미 | 시맨틱 레이어로 통합 |
| 컬럼명만 있고 의미 설명 없음 | 메타데이터·[[Ontology]] 보유 |
| 중복·이상치·결측 그대로 | 정제·표준화 완료 |
| 문서 간 관계 미상 | [[Knowledge Graph]]로 명시 |
| 청크 추출만 됨 | 청크 + 출처 + 시점 + 권한 |

## 4가지 핵심 축

### 1. Schema & Ontology

- 컬럼·필드·문서 타입에 의미가 부여돼 있음.
- [[Ontology]]로 도메인 개념의 관계가 정의돼 있음.

### 2. Quality & Lineage

- 결측·중복·표기 통일.
- 어디서 왔는지(lineage), 언제 갱신됐는지(freshness).

### 3. Access Layer

- 단순 SQL이 아니라 **시맨틱 레이어**(dbt, Cube, LookML) 혹은 [[Knowledge Graph|KG]].
- LLM이 컬럼 의미를 추측하지 않고 정의된 메트릭을 호출.

### 4. Permission & PII

- 데이터마다 누가 접근 가능한지.
- PII는 자동 마스킹 / 별도 권한.

## 왜 RAG·Agent에 중요한가

- 청크가 의미 단위로 잘려 있고 메타데이터가 잘 붙어 있어야 → [[청킹(Chunking)|청킹]]과 [[Hybrid Search]] 성능 좋음.
- 수치 질의는 RAG가 약함 → [[Knowledge Graph|KG]] + Text-to-Cypher로 정확도 ↑.
- 권한 정보가 있어야 [[Multi Agent]] 시스템에서 사용자별 격리 가능.

## 준비 절차 (단순화)

```
[1] 비즈니스 질문 수집      ── "AI로 답하고 싶은 것은?"
[2] 필요한 데이터 식별      ── 정형·비정형 모두
[3] Ontology 설계            ── 엔티티·관계·메트릭
[4] 정제 & 표준화            ── 결측·중복·표기
[5] KG / Vector / Catalog 구축
[6] 시맨틱 레이어 API 노출
[7] Agent가 호출
```

## 흔한 함정

- "RAG가 답을 못 한다" → 모델 탓을 하지만 대부분 데이터·청킹 문제.
- 사일로 그대로 두고 KG만 얹기 → 결국 사일로의 합집합일 뿐.
- Ontology를 한 번 잡으면 변하지 않는다고 생각 → 도메인 변화에 따라 진화시켜야 함.

## 관련

- [[Ontology]] · [[Knowledge Graph]] · [[GraphRAG]].
- [[청킹(Chunking)]] · [[임베딩(Embedding)]] — 비정형 측 준비.
- [[Hybrid Search]] · [[Reranking]] — 준비된 데이터를 끌어내는 검색.
