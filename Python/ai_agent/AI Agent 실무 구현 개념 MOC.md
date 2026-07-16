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
- [[Async Resource Lifecycle]]
- [[dataclass]]
- [[Pydantic]]

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
