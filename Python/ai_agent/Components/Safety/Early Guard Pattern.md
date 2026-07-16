---
title: Early Guard Pattern
created: 2026-06-26
tags:
  - ai-agent
  - guardrail
  - architecture
---

# Early Guard Pattern

Early Guard Pattern은 LLM 호출이나 tool 실행 전에 cheap deterministic check를 먼저 수행하는 패턴이다.

## 목적

- 처리 범위 밖 요청을 빠르게 제외한다.
- 명백한 위험 요청을 차단한다.
- 이미 정답이 확정된 shortcut을 먼저 처리한다.
- 불필요한 토큰 비용과 tool 실행을 줄인다.

## 예시

- 구조 변경 요청은 옵션 변경 handler가 아니라 planner로 보낸다.
- 이미 실패가 학습된 선택지는 다시 시도하지 않는다.
- 필수 입력이 없으면 LLM 호출 전에 사용자 안내를 만든다.
- 특정 키워드가 명확하면 rule-based shortcut으로 처리한다.

## 설계 원칙

Early guard는 복잡한 추론을 하면 안 된다. 빠르고 결정적인 조건만 담당하고, 애매한 판단은 LLM이나 상위 planner에 맡긴다.

관련: [[Guardrails]], [[Fallback]], [[Lightweight Intent Handler]]
