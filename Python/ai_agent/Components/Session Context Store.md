---
title: Session Context Store
created: 2026-06-26
tags:
  - ai-agent
  - memory
  - session
---

# Session Context Store

Session Context Store는 agent가 현재 대화나 workflow의 상태를 `session_id` 기준으로 저장하는 구조이다.

## 저장 대상

- 현재 UI/workflow state
- 최근 tool 실행 결과
- 사용자 preference lock
- turn context
- metadata snapshot
- pending action

## 왜 필요한가

LLM 호출은 stateless에 가깝지만, 실제 제품은 세션 상태를 기억해야 한다. Session Context Store는 UI, tool, agent loop 사이에서 같은 현재 상태를 공유하게 해준다.

## 구현 선택지

- in-memory dict: 단순하고 빠르지만 프로세스 종료 시 사라진다.
- Redis: 여러 서버 인스턴스가 상태를 공유할 수 있다.
- DB: 장기 보존과 감사 로그에 유리하다.
- checkpointer: [[LangGraph]] 실행 상태 복원에 적합하다.

관련: [[Memory]], [[LangGraph Checkpointer]], [[User Preference Locking]]
