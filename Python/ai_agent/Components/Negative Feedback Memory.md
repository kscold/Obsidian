---
title: Negative Feedback Memory
created: 2026-06-26
tags:
  - ai-agent
  - memory
  - self-healing
---

# Negative Feedback Memory

Negative Feedback Memory는 과거에 실패하거나 부적합하다고 판정된 선택을 기억해 같은 실수를 반복하지 않게 하는 메모리 패턴이다.

## 예시

- 특정 데이터셋에서 분류 타깃으로 부적합했던 컬럼
- 실행 오류를 반복해서 만든 tool argument
- 사용자에게 거절당한 추천 전략
- 비용이 너무 컸던 retrieval route

## 왜 필요한가

Agent는 매번 새로 추론하면 같은 실패를 반복할 수 있다. 실패 원인을 구조화해서 저장하면 다음 추천, planning, prompt에서 해당 선택지를 낮추거나 제외할 수 있다.

## 키 설계

세션 단위로 저장할지, 데이터셋/문서/사용자 단위로 저장할지 결정해야 한다. 같은 실패가 여러 세션에서 재사용될 가치가 있으면 session보다 안정적인 signature를 키로 쓰는 것이 좋다.

## 주의

Negative feedback은 영구 금지가 아니라 강한 힌트로 다루는 편이 안전하다. 데이터가 바뀌거나 사용자가 명시적으로 원하면 다시 시도할 수 있어야 한다.

관련: [[Memory]], [[Self-Improving Agent]], [[Fail-soft]]
