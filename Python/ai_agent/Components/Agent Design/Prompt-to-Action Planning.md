---
title: Prompt-to-Action Planning
created: 2026-06-26
tags:
  - ai-agent
  - planning
  - structured-output
---

# Prompt-to-Action Planning

Prompt-to-Action Planning은 LLM에게 자연어 답변이 아니라 실행 가능한 action 목록을 만들게 하는 패턴이다.

## 핵심 아이디어

LLM은 "무엇을 해야 하는지"를 계획하고, 실제 실행은 검증된 tool/action layer가 담당한다. 즉 LLM 출력은 최종 결과가 아니라 중간 계획이다.

## 출력 예시

```json
[
  {
    "action_type": "ADD_NODE",
    "node_type": "visualization",
    "insert_after": "last"
  },
  {
    "action_type": "SET_OPTION",
    "target": "new_node",
    "options": {
      "x": "age",
      "y": "score"
    }
  }
]
```

## 파이프라인

1. [[Prompt Context Builder]]가 현재 상태, 사용 가능한 action, 계약을 prompt에 넣는다.
2. LLM이 JSON action plan을 반환한다.
3. parser가 action list를 구조화한다.
4. [[Contract Guardrail Pipeline]]이 action과 option을 검증한다.
5. tool/action executor가 실제 상태를 변경한다.
6. [[Response Builder Pattern]]이 사용자 응답을 만든다.

## 장점

- 자연어 응답보다 실행 가능성이 높다.
- action 단위로 검증, 기록, rollback을 설계할 수 있다.
- partial success와 blocked action을 분리하기 쉽다.

## 주의

LLM이 만든 action plan은 신뢰 경계 밖에 있다. 반드시 schema validation, contract validation, allowlist 검사를 거쳐야 한다.

관련: [[Structured Output]], [[Workflow Action]], [[LangChain @tool]]
