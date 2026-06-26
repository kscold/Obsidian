---
title: json.loads
created: 2026-06-26
tags:
  - python
  - json
---

# json.loads

`json.loads(text)`는 JSON 문자열을 Python 객체로 역직렬화한다.

## 실무에서의 쓰임

- LLM이 반환한 JSON modification을 list/dict로 바꾼다.
- action payload나 tool argument를 다시 dict로 복원해 상태를 갱신한다.
- 옵션 값이 JSON 문자열로 들어오는 경우 list 형태로 정규화한다.

## 주의

LLM 응답은 순수 JSON이 아닐 수 있다. 그래서 실무에서는 JSON 부분만 먼저 추출한 뒤 `json.loads`에 넘기는 보조 파서를 두는 경우가 많다.

관련: [[json.dumps]], [[Structured Output]], [[Prompt Context Builder]]
