---
title: Action Parser
created: 2026-06-26
tags:
  - ai-agent
  - structured-output
  - parser
---

# Action Parser

Action Parser는 LLM이 만든 JSON, tool call, markdown code block 등에서 실제 실행 계획을 추출하고 표준 action 형식으로 정규화하는 레이어이다.

## 왜 필요한가

LLM에게 "JSON만 출력하라"고 해도 설명 문장, 코드 fence, 단일 dict, list, null이 섞여 나올 수 있다. Action Parser는 이런 흔들림을 흡수하고 executor가 기대하는 입력으로 바꾼다.

## 처리할 것

- JSON code block 제거
- 단일 dict를 list로 감싸기
- action type allowlist 검사
- 필수 필드 확인
- 복잡도가 높은 action은 상위 planner로 fallback
- parse 실패는 명확한 blocked/fallback signal로 반환

## 예시

```python
def parse_actions(value) -> list[dict] | None:
    parsed = extract_json_payload(value)
    if isinstance(parsed, dict):
        return [parsed]
    if isinstance(parsed, list):
        return [item for item in parsed if isinstance(item, dict)]
    return None
```

## 주의

Parser가 너무 많은 의미 보정을 하면 LLM의 잘못된 계획을 조용히 실행하게 된다. 구조 정규화는 parser가 하고, 의미 검증은 [[Contract Guardrail Pipeline]]이 맡는 편이 좋다.

관련: [[Prompt-to-Action Planning]], [[Structured Output]], [[json.loads]]
