---
title: json.dumps
created: 2026-06-26
tags:
  - python
  - json
---

# json.dumps

`json.dumps(obj)`는 Python 객체를 JSON 문자열로 직렬화한다.

## 자주 쓰는 옵션

- `ensure_ascii=False`: 한글을 `\uXXXX`로 escape하지 않고 그대로 남긴다.
- `default=str`: JSON으로 바꿀 수 없는 객체를 문자열로 fallback한다.

## 실무에서의 쓰임

- LLM prompt와 로그에서 옵션 dict를 사람이 읽을 수 있게 출력한다.
- action payload나 tool argument를 문자열로 저장한다.
- upstream blocks를 tool 입력으로 넘길 때 JSON 문자열로 만든다.

관련: [[json.loads]], [[Prompt Context Builder]], [[Workflow Action]]
