---
title: User Preference Locking
created: 2026-06-26
tags:
  - ai-agent
  - memory
  - personalization
---

# User Preference Locking

User Preference Locking은 사용자가 명시적으로 선택한 값이나 선호를 agent가 다음 턴에서도 보존하는 메모리 패턴이다.

## 왜 필요한가

Agent가 매 턴 최적화를 다시 수행하면 사용자가 직접 바꾼 설정을 AI가 덮어쓸 수 있다. 이 문제를 막기 위해 "사용자가 고른 값"을 별도 lock으로 저장하고, 다음 planning/prompt/tool call에서 변경 금지 조건으로 넣는다.

## 저장할 수 있는 것

- tool option
- 선택한 모델/전략
- 제외해야 할 데이터 컬럼
- 선호하는 출력 형식
- 사용자가 수동으로 고친 workflow 상태

## 설계 원칙

- 사용자 명시값은 AI 추천보다 우선한다.
- 사용자가 다시 변경을 요청하면 lock도 새 값으로 갱신한다.
- 삭제된 대상의 lock은 함께 제거한다.
- prompt에는 짧고 명확한 금지 규칙으로 넣는다.

관련: [[Memory]], [[Session Context Store]], [[Configuration Merge Pipeline]]
