---
title: Profile-based Input Sanitization
created: 2026-06-26
tags:
  - ai-agent
  - data
  - guardrail
---

# Profile-based Input Sanitization

Profile-based Input Sanitization은 데이터 프로파일을 이용해 AI가 만든 입력값을 실행 전에 정리하는 기법이다.

## 예시

- ID처럼 보이는 컬럼을 feature에서 제거한다.
- 고카디널리티 범주형 컬럼을 경고한다.
- 결측이 많은 컬럼을 자동 선택에서 낮춘다.
- 분류 타깃으로 부적합한 컬럼을 후보에서 제외한다.

## 왜 필요한가

LLM은 컬럼 이름만 보고 그럴듯한 feature나 target을 고를 수 있다. 하지만 실제 데이터 분포를 보면 모델링에 부적합한 경우가 많다. 데이터 프로파일 기반 sanitization은 이런 선택을 실행 전에 줄인다.

## 주의

사용자가 명시적으로 고른 값은 조용히 제거하지 않는 편이 좋다. 대신 [[Contract Guardrail Pipeline]]이나 [[Response Builder Pattern]]에서 이유를 설명하고 차단하는 방식이 더 투명하다.

관련: [[Dataset Profiling]], [[Data-aware Prompting]], [[Guardrails]]
