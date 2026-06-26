---
title: Specialist Agent Pattern
created: 2026-06-26
tags:
  - ai-agent
  - architecture
  - specialist
---

# Specialist Agent Pattern

Specialist Agent Pattern은 범용 LLM이 만든 계획을 도메인별 좁은 전문가 로직이나 전문가 agent로 다시 보정하는 패턴이다.

## 왜 필요한가

범용 LLM은 큰 방향을 잘 잡지만, 특정 도구의 옵션, 데이터 타입 제약, 실행 환경의 물리 조건까지 항상 정확히 알지는 못한다. Specialist는 좁은 영역의 규칙과 경험을 바탕으로 계획을 실행 가능한 형태로 다듬는다.

## 사용 방식

- Planner가 초안을 만든다.
- Specialist가 누락된 옵션을 채우거나 위험한 값을 경고한다.
- 사용자가 명시한 값은 우선순위가 높다.
- 최종 결과는 [[Contract Guardrail Pipeline]]에서 다시 검증한다.

## 주의

Specialist가 사용자 명시값을 조용히 바꾸면 신뢰가 깨진다. 명시값이 위험하면 자동 치환보다 차단/설명이 낫다.

관련: [[Sub-LLM as Tool]], [[Contract-first Workflow]], [[Human-in-the-loop]]
