---
title: LangGraph 개념 MOC
created: 2026-06-22
tags:
  - python
  - ai-agent
  - langgraph
  - moc
---

# LangGraph 개념 MOC

## 프레임워크

- [[LangGraph]]
- [[LangGraph StateGraph]]
- [[LangGraph create_react_agent]]
- [[LangGraph 문법 치트시트]]

## 핵심 개념

- [[Agent 4대 패턴]]
- [[LangGraph State]]
- [[LangGraph Node]]
- [[LangGraph Edge]]
- [[LangGraph START]]
- [[LangGraph END]]
- [[invoke]]

## Workflow와 Tool

- [[Workflow Node vs Tool]]
- [[LangChain @tool]]
- [[LLM Tool Selection]]
- [[LangGraph ToolNode]]
- [[Tool Calling]]

## RAG 패턴

- [[Retrieve-Generate 패턴]]
- [[RAG(Retrieval-Augmented Generation)]]
- [[Agentic RAG]]

## 아키텍처

- [[LangGraph 워크플로우 아키텍처]]
- [[Agent vs Workflow]]
- [[Agent Graph]]
- [[Agent System Topologies]]
- [[Serial Agent Pipeline]]
- [[Parallel Agent Fan-out]]
- [[Sub-LLM as Tool]]
- [[Agent as Tool]]
- [[External Information MAS]]

## 신뢰성 / 메모리 / 운영

- [[Memory]]
- [[LangGraph 메모리 상태 관리]]
- [[LangGraph Checkpointer]]
- [[LangGraph InMemorySaver]]
- [[LangGraph SqliteSaver]]
- [[LangGraph PostgresSaver]]
- [[LangGraph thread_id]]
- [[LangGraph Store]]
- [[LangGraph InMemoryStore]]
- [[LangGraph namespace]]
- [[LangGraph 운영용 메모리 저장소]]
- [[LangGraph recursion_limit]]
- [[GraphRecursionError]]
- [[Loop Control]]
- [[Fallback]]
- [[Human-in-the-loop]]
- [[LangGraph interrupt]]
- [[Evaluation]]
- [[LangSmith]]
- [[AI 평가 지표]]
- [[분류 평가 지표]]
- [[회귀 평가 지표]]
- [[텍스트 생성 평가 지표]]
- [[BERTScore]]
- [[LLM-as-Judge]]
- [[Observability]]

## 핵심 질문

- 고정 실행 단계인가? → [[LangGraph Node]]
- LLM이 필요할 때 선택하는 기능인가? → [[LangChain @tool]]
- 도구를 쥐어준 뒤 누가 도구를 고르는가? → [[LLM Tool Selection]]
- 노드들이 공유하는 데이터는 어디에 담기는가? → [[LangGraph State]]
- 실행 순서는 어디서 정하는가? → [[LangGraph Edge]]
- 그래프 시작점은? → [[LangGraph START]]
- 그래프 정상 종료 지점은? → [[LangGraph END]]
- 검색 후 생성 구조는 무엇인가? → [[Retrieve-Generate 패턴]]
- 입력을 넣고 실제 실행하는 메서드는? → [[invoke]]
- 하위 전문가 LLM을 도구처럼 쓰는 구조는? → [[Sub-LLM as Tool]]
- 기본 ReAct 도구 호출 agent를 빠르게 만드는 함수는? → [[LangGraph create_react_agent]]
- 여러 에이전트를 정해진 순서대로 실행하는 구조는? → [[Serial Agent Pipeline]]
- 여러 에이전트를 동시에 실행 후 합치는 구조는? → [[Parallel Agent Fan-out]]
- 그래프 실행 상태를 이어가려면? → [[LangGraph Checkpointer]]
- 세션별 대화 상태를 구분하는 값은? → [[LangGraph thread_id]]
- 메모리 기반 checkpointer 실습 구현은? → [[LangGraph InMemorySaver]]
- 파일 기반으로 checkpoint를 보존하려면? → [[LangGraph SqliteSaver]]
- 운영용 DB 기반 checkpoint는? → [[LangGraph PostgresSaver]]
- 사람이 승인할 때까지 그래프를 멈추는 기능은? → [[LangGraph interrupt]]
- 여러 대화에서 재사용할 기억을 저장하려면? → [[LangGraph Store]]
- RAM 기반 Store 실습 구현은? → [[LangGraph InMemoryStore]]
- Store의 장기 기억을 분류하는 기준은? → [[LangGraph namespace]]
- 운영에서 메모리를 DB에 저장하려면? → [[LangGraph 운영용 메모리 저장소]]
- 무한 루프를 막으려면? → [[Loop Control]]
- END 없는 반복 그래프를 강제로 끊으려면? → [[LangGraph recursion_limit]]
- 반복 제한에 걸렸을 때 나는 에러는? → [[GraphRecursionError]]
- LangGraph 실행을 trace로 보고 싶다면? → [[LangSmith]]
