---
title: Response Builder Pattern
created: 2026-06-26
tags:
  - ai-agent
  - response
  - architecture
---

# Response Builder Pattern

Response Builder Pattern은 내부 실행 결과를 사용자에게 보여줄 안정적인 응답 형태로 바꾸는 레이어이다.

## 왜 필요한가

Agent 내부에는 action, warning, blocked reason, fallback signal, sentinel 같은 기계적인 상태가 많다. 이 상태를 그대로 사용자에게 보여주면 응답이 거칠고 불안정해진다. Response Builder는 내부 상태를 숨기고, 사용자가 이해할 수 있는 결과와 다음 행동으로 바꾼다.

## 주로 처리하는 상태

- 성공: 어떤 변경이 적용되었는지 설명한다.
- no-op: 이미 원하는 상태라 바꿀 것이 없음을 말한다.
- blocked: 정책이나 계약 때문에 적용하지 않았음을 말한다.
- partial success: 일부만 적용되고 일부는 차단되었음을 분리한다.
- fallback: 더 넓은 planner가 필요하다는 신호를 상위 레이어로 넘긴다.

## 설계 원칙

- 내부 marker나 sentinel을 사용자에게 노출하지 않는다.
- 실패와 "변경 없음"을 구분한다.
- 구조 변경이 필요한 경우 좁은 handler가 억지로 해결하지 않는다.
- 최종 응답은 [[Agent 응답 정규화]] 스키마에 맞춘다.

관련: [[Agent 응답 정규화]], [[Fallback]], [[str.removeprefix]]
