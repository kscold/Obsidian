---
title: AI Agent 실무 구현 개념 MOC
created: 2026-06-26
tags:
  - ai-agent
  - moc
  - implementation
---

# AI Agent 실무 구현 개념 MOC

실무 AI Agent 구현에서 반복해서 등장하는 프레임워크, 설계 패턴, Python 개념을 모은 인덱스이다.

## 검색 / RAG

- [[Hybrid RAG Search]]
- [[BM25]]
- [[BGE-M3]]
- [[Qdrant]]
- [[Reciprocal Rank Fusion]]
- [[Agentic Search Tool]]
- [[GraphRAG]]
- [[Lexical Graph RAG]]
- [[Query Transformation]]
- [[Grounded Generation]]
- [[Graph Data Modeling]]
- [[Graph Traversal Retrieval]]
- [[Retrieval Evaluation]]
- [[임베딩(Embedding)]]
- [[벡터 데이터베이스]]
- [[코사인 유사도]]
- [[구조화 검색 채널]]
- [[TF-IDF]]
- [[한국어 토크나이징과 BM25]]

## 벡터 인덱스 / Qdrant 실무 (`Database/Vector/`)

- [[ANN(근사 최근접 이웃)]]
- [[Small World 그래프]]
- [[HNSW]]
- [[HNSW 파라미터]]
- [[Recall과 ef 튜닝]]
- [[Qdrant Point와 Payload]]
- [[Qdrant query_points]]
- [[Qdrant 메타데이터 필터]]
- [[Payload Index]]
- [[Pre-filtering vs Post-filtering]]
- [[score_threshold]]
- [[Qdrant 인덱스 재빌드 전략]]

## 데이터베이스 기초 (`Database/`)

전체 지도는 [[데이터베이스 MOC]]. 폴더는 `Foundations · SQL · NoSQL · Index · Vector · Patterns`로 나뉜다.

- [[쿼리(Query)]]
- [[SQL]] · [[SQL 실행 순서]] · [[JOIN]] · [[트랜잭션(ACID)]]
- MongoDB → [[MongoDB MOC]] (문서 모델 · 쿼리/갱신 연산자 · CRUD · 커서 · 집계 · 인덱스 · 모델링 · 예외 · mongosh)
- [[인덱스(Index)]] · [[B-tree]] · [[복합 인덱스]] · [[유니크 인덱스]] · [[풀 스캔(Full Scan)]] · [[explain과 실행 계획]]
- [[RDBMS vs Document DB]]

## 저장소 실무 패턴 (`Database/Patterns/` · 메서드·코드 예시)

- [[MongoDB CRUD 메서드]] · [[Qdrant 메서드 사전]] · [[LangChain VectorStore 메서드]]
- [[AI Agent 컬렉션 지도]]
- [[Mongo 스키마 레지스트리]]
- [[Repository 패턴과 Port]]
- [[Snapshot 캐시와 무효화]]
- [[Degraded와 fail-soft 저장소]]
- [[Seed 배포와 멱등 동기화]]
- [[임베딩 모델 로딩 패턴]]

## LangGraph / Tool

- [[LangGraph Agent Loop]]
- [[LangGraph State Reducer]]
- [[LangGraph StateGraph]]
- [[LangGraph ToolNode]]
- [[LangGraph Command]]
- [[LangGraph Send]]
- [[LangGraph Durable Execution]]
- [[LangChain @tool]]
- [[Tool Observability Wrapping]]
- [[Tool Execution Policy]]
- [[Concurrent Tool Execution]]
- [[SSE 기반 Agent Streaming]]

## 계약 / 가드레일

- [[Contract-first Workflow]]
- [[Block Contract]]
- [[Block Manifest]]
- [[Contract Guardrail Pipeline]]
- [[Guardrails]]
- [[Configuration Merge Pipeline]]
- [[Lightweight Intent Handler]]
- [[Rule-based Shortcut]]
- [[Prompt Context Builder]]
- [[Context Engineering]]
- [[Prompt-to-Action Planning]]
- [[Action Parser]]
- [[Capability Catalog]]
- [[Specialist Agent Pattern]]
- [[Response Builder Pattern]]
- [[Early Guard Pattern]]
- [[Advisory Short-circuit]]
- [[Pre-validation Normalizer]]
- [[Post-merge Normalizer]]
- [[Profile-based Input Sanitization]]
- [[No-op Change Guard]]
- [[Workflow Action]]
- [[Agent 응답 정규화]]

## 데이터 프로파일 / 세션 메모리

- [[Dataset Profiling]]
- [[Data-aware Prompting]]
- [[User Preference Locking]]
- [[Session Context Store]]
- [[Negative Feedback Memory]]

## Python 구현 패턴

- [[Protocol]]
- [[TypedDict]]
- [[Annotated]]
- [[Literal]]
- [[ContextVar]]
- [[field(default_factory)]]
- [[Double-Checked Locking]]
- [[Lazy Initialization]]
- [[Singleton Pattern]]
- [[Registry Pattern]]
- [[Factory Pattern]]
- [[Mixin]]
- [[__getattr__ Lazy Import]]
- [[object.__setattr__]]
- [[functools.wraps]]
- [[threading.RLock]]
- [[copy.deepcopy]]
- [[json.dumps]]
- [[json.loads]]
- [[str.removeprefix]]
- [[async-await]]
- [[asyncio.gather와 병렬 실행]]
- [[Async Resource Lifecycle]]
- [[dataclass]]
- [[Enum]]
- [[TypeVar와 제네릭]]
- [[Pydantic]]
- [[Pydantic v2 메서드 사전]]
- [[collections.defaultdict]]
- [[OrderedDict와 LRU 캐시]]
- [[hashlib과 결정적 ID]]

## LLM 런타임 / sLLM · Ollama

- [[Ollama]]
- [[sLLM(소형 LLM) 운용]]
- [[컨텍스트 윈도우와 num_ctx]]
- [[LLM 샘플링 파라미터]]
- [[LLM 호출 원장과 예산]]
- [[LLM Provider 추상화]]
- [[LLM Routing]]
- [[Cost와 Token]]

## 구조화 출력 (JSON 계약)

- [[Structured Output]]
- [[with_structured_output]]
- [[Structured Output 정규화]]
- [[JSON Schema]]
- [[Pydantic v2 메서드 사전]]

## 운영 / 관측

- [[Observability]]
- [[AI Pipeline Error Normalization]]
- [[Fail-soft]]
- [[AI Workflow 테스트 하니스]]
- [[Evaluation]]
- [[Grounded Generation]]
- [[Retrieval Evaluation]]

## Runtime / 상호운용성

- [[Prompt Caching]]
- [[LLM Routing]]
- [[Agent Middleware]]
- [[MCP(Model Context Protocol)]]
- [[MCP Security]]
- [[A2A 프로토콜]]
- [[SKILL]]
