---
title: Lightweight Intent Handler
created: 2026-06-26
tags:
  - ai-agent
  - intent
  - architecture
---

# Lightweight Intent Handler

Lightweight Intent Handler는 모든 요청을 전체 agent loop로 보내지 않고, 범위가 좁고 결과가 명확한 요청만 빠른 경로로 처리하는 아키텍처 패턴이다.

## 언제 쓰나

- 사용자의 intent가 좁고 명확할 때
- 전체 planning loop를 돌리기에는 비용과 지연이 클 때
- 수행할 mutation 종류가 제한되어 있을 때
- 실패하면 상위 planner로 fallback할 수 있을 때

## 기본 흐름

1. intent classifier가 요청을 좁은 handler로 보낸다.
2. [[Early Guard Pattern]]이 이 handler가 처리 가능한 요청인지 확인한다.
3. [[Prompt Context Builder]]가 필요한 사실과 제약을 모은다.
4. LLM 또는 rule-based planner가 작은 실행 계획을 만든다.
5. [[Contract Guardrail Pipeline]]이 실행 가능성을 검증한다.
6. [[Response Builder Pattern]]이 사용자 응답을 만든다.

## 장점

- 전체 agent loop보다 빠르다.
- 잘 정의된 작업에서 예측 가능성이 높다.
- tool call과 prompt context를 작게 유지할 수 있다.
- 실패 시 넓은 planner로 넘기는 [[Fallback]] 전략과 잘 맞는다.

## 주의

좁은 handler가 구조 변경, 복합 작업, 모호한 요청까지 억지로 처리하면 오히려 품질이 나빠진다. 처리 범위를 작게 유지하고, 경계 밖 요청은 과감하게 상위 agent loop로 넘겨야 한다.

관련: [[Intent Classification]], [[LangGraph Agent Loop]], [[Fallback]]
