---
title: No-op Change Guard
created: 2026-06-26
tags:
  - ai-agent
  - guardrail
  - workflow
---

# No-op Change Guard

No-op Change Guard는 "변경 요청처럼 보이지만 실제 상태가 달라지지 않는 action"을 차단하는 검증층이다.

## 왜 필요한가

LLM은 사용자의 요청을 처리한 것처럼 보이기 위해 기존 값과 동일한 설정을 다시 반환할 수 있다. 이때 action을 그대로 적용하면 사용자는 시스템이 뭔가 바꿨다고 착각하고, 디버깅 로그도 불필요하게 더러워진다.

## 검사 기준

- raw value가 달라졌는가
- 정규화 후 의미가 달라졌는가
- 사용자 요청이 실제 변경을 요구했는가
- presentation-only 필드만 바뀐 것은 아닌가

## 응답 원칙

No-op은 실패가 아니다. "이미 원하는 상태라 변경하지 않았다"는 별도 상태로 사용자에게 설명하는 것이 좋다.

관련: [[Configuration Merge Pipeline]], [[Response Builder Pattern]], [[Guardrails]]
