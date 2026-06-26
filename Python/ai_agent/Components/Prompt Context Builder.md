---
title: Prompt Context Builder
created: 2026-06-26
tags:
  - ai-agent
  - prompt
  - architecture
---

# Prompt Context Builder

Prompt Context Builder는 LLM 호출에 필요한 사실, 제약, 현재 상태, 출력 형식을 하나의 입력 컨텍스트로 조립하는 레이어이다.

## 역할

- 현재 상태를 LLM이 읽기 쉬운 형태로 요약한다.
- [[Block Contract]]나 tool schema 같은 실행 제약을 포함한다.
- [[Data-aware Prompting]]처럼 데이터셋 신호를 넣는다.
- 출력 형식을 [[Structured Output]]으로 제한한다.
- 관련 지식은 [[Hybrid RAG Search]]나 rule retrieval로 보강한다.

## 왜 분리하나

Prompt 조립을 handler 안에 흩어두면 테스트가 어렵고, 어떤 정보가 LLM 판단에 들어갔는지 추적하기 힘들다. Context Builder를 분리하면 prompt의 입력 재료, 우선순위, 토큰 예산을 독립적으로 관리할 수 있다.

## 설계 원칙

- 사실과 지시를 구분한다.
- 현재 상태와 사용자 요청을 모두 넣는다.
- LLM이 바꾸면 안 되는 값은 명확히 표시한다.
- 실패 가능한 보조 컨텍스트는 [[Fail-soft]]로 다룬다.
- 출력 스키마를 짧고 엄격하게 둔다.

관련: [[Prompt Engineering]], [[Structured Output]], [[Data-aware Prompting]]
