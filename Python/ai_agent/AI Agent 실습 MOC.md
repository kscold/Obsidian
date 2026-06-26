---
title: AI Agent 실습 MOC
created: 2026-06-22
tags:
  - python
  - ai-agent
  - moc
---

# AI Agent 실습 MOC

> 이 노트는 실습 중 나온 내용을 개념 노트로 연결하는 임시 인덱스이다. 일반 개념 인덱스는 [[LangGraph 개념 MOC]]를 사용한다.

## 실무 구현 인덱스

- [[AI Agent 실무 구현 개념 MOC]]

## LangGraph 개념

- [[LangGraph StateGraph]]
- [[LangGraph State]]
- [[LangGraph Node]]
- [[LangGraph Edge]]
- [[LangGraph 워크플로우 아키텍처]]

## Workflow와 Tool 구분

- [[Workflow Node vs Tool]]
- [[LangChain @tool]]
- [[LangGraph ToolNode]]
- [[Tool Calling]]

## AI Workflow 구현 패턴

- [[Contract-first Workflow]]
- [[AI Workflow 생성 파이프라인]]
- [[Agent 응답 정규화]]
- [[Lightweight Intent Handler]]
- [[Prompt Context Builder]]
- [[Specialist Agent Pattern]]
- [[Response Builder Pattern]]
- [[Early Guard Pattern]]
- [[Pre-validation Normalizer]]
- [[Post-merge Normalizer]]
- [[Profile-based Input Sanitization]]
- [[Configuration Merge Pipeline]]
- [[Contract Guardrail Pipeline]]
- [[Block Contract]]
- [[Block Manifest]]
- [[No-op Change Guard]]
- [[Workflow Action]]
- [[블록 Alias Map]]
- [[캔버스 적용 오케스트레이터]]
- [[SSE 기반 Agent Streaming]]
- [[AI Pipeline Error Normalization]]
- [[AI Workflow 테스트 하니스]]

## 데이터 프로파일 / 세션 메모리

- [[Dataset Profiling]]
- [[Data-aware Prompting]]
- [[User Preference Locking]]
- [[Session Context Store]]
- [[Negative Feedback Memory]]

## 검색 / RAG 구현

- [[Hybrid RAG Search]]
- [[BM25]]
- [[BGE-M3]]
- [[Qdrant]]
- [[Reciprocal Rank Fusion]]
- [[Agentic Search Tool]]

## RAG 흐름

- [[Retrieve-Generate 패턴]]
- [[RAG(Retrieval-Augmented Generation)]]
- [[RAG Agent]]
- [[OCR]]
- [[VLM]]
- [[PyMuPDF]]

## 외부 정보 / DB 연동

- [[External Information MAS]]
- [[로컬 우선 정보 수집 MAS]]
- [[SQLite 데이터 소스]]
- [[LangGraph Conditional Fan-out]]
- [[Fallback]]

## 핵심 키워드

- State
- Node
- Edge
- StateGraph
- `compile()`
- `graph.invoke()`
- Retrieve
- Generate
- RAG
- LLM Workflow
- `@tool`
- ToolNode
- Conditional Fan-out
- Local-first
- SQLite DB
- CSV Agent
- Web Agent

## 평가

- [[Evaluation]]
- [[LangSmith]]
- [[AI 평가 지표]]
- [[분류 평가 지표]]
- [[회귀 평가 지표]]
- [[텍스트 생성 평가 지표]]
- [[다수결 평가]]
- [[라벨 정규화]]
- [[BLEU]]
- [[ROUGE]]
- [[BERTScore]]
- [[LLM-as-Judge]]
