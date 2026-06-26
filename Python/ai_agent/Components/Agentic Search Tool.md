---
title: Agentic Search Tool
created: 2026-06-26
tags:
  - ai-agent
  - tool-calling
  - rag
---

# Agentic Search Tool

Agentic Search Tool은 LLM이 필요할 때 직접 검색 도구를 호출해 정보를 가져오게 하는 방식이다.

모든 RAG 컨텍스트를 프롬프트에 미리 넣는 방식은 토큰을 많이 쓰고, 좁은 질문에는 정보가 너무 넓다. 도구 검색은 "필요할 때만 찾아보기"에 가깝다.

## 예

- `search_block_recipe`
- `inspect_block_options`
- `find_similar_pipelines`

이 도구들은 [[LangChain @tool]]로 등록되고, [[LangGraph ToolNode]]가 실행한다.

## Pre-pack RAG와 비교

| 방식 | 장점 | 단점 |
|---|---|---|
| Pre-pack RAG | LLM이 처음부터 전체 맥락을 봄 | 토큰 비용 큼 |
| Agentic Search Tool | 필요한 정보만 검색 | LLM이 도구 호출을 잘 선택해야 함 |

## 설계 팁

- 도구명은 행동이 드러나게 짓는다.
- 입력 스키마는 작게 유지한다.
- 실패 시 예외 대신 읽을 수 있는 오류 문자열을 반환하면 [[Fail-soft]]에 유리하다.
- 검색 도구는 캔버스를 변경하지 않는 READ 도구로 분류한다.

## 한 줄 정리

Agentic Search Tool은 **LLM이 필요한 순간에 RAG 색인을 직접 조회하게 하는 도구 기반 검색 패턴**이다.

## 관련

- [[Tool Calling]]
- [[LangChain @tool]]
- [[Hybrid RAG Search]]
- [[LLM Tool Selection]]
