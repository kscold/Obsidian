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
