---
title: Fail-soft
created: 2026-06-26
tags:
  - ai-agent
  - reliability
  - fallback
---

# Fail-soft

Fail-soft는 일부 보조 기능이 실패해도 전체 시스템을 중단하지 않고, 가능한 기본 흐름을 계속 진행하는 설계 원칙이다.

AI Agent에서는 RAG, sub-agent, graph search, LLM critic 같은 부가 계층이 실패할 수 있다. 이때 핵심 워크플로우 생성 자체까지 죽이면 사용자 경험이 나빠진다.

## 예

- Neo4j 연결 실패 -> graph 채널만 끄고 vector + [[BM25]]로 검색
- sub-agent 실패 -> `noop` 결과 반환
- RAG 검색 실패 -> core block set fallback
- 도구 wrapping 실패 -> warning만 남기고 원래 도구 실행

## fail-soft와 fail-closed

| 방식 | 의미 | 사용처 |
|---|---|---|
| fail-soft | 실패해도 기본 흐름 계속 | 추천 품질 보조 기능 |
| fail-closed | 실패하면 차단 | 보안, destructive action, 권한 |

## 한 줄 정리

Fail-soft는 **부가 기능 실패를 흡수하고 핵심 사용자 흐름을 계속 살리는 안정성 패턴**이다.

## 관련

- [[Fallback]]
- [[Observability]]
- [[Guardrails]]
- [[Hybrid RAG Search]]
