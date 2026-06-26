---
title: ContextVar
created: 2026-06-26
tags:
  - python
  - async
  - observability
---

# ContextVar

`ContextVar`는 async 환경에서 현재 실행 흐름별로 값을 안전하게 보관하는 변수이다.

전역 변수처럼 보이지만, asyncio task마다 값이 분리되어 세션 ID나 trace ID를 전파할 때 유용하다.

## 예

```python
from contextvars import ContextVar

turn_id_var = ContextVar("turn_id", default="")

token = turn_id_var.set("abc123")
try:
    ...
finally:
    turn_id_var.reset(token)
```

## 왜 전역 변수 대신 쓰나

서버는 여러 요청을 동시에 처리한다. 일반 전역 변수에 `turn_id`를 저장하면 다른 요청의 값과 섞일 수 있다.

`ContextVar`는 같은 async 호출 체인 안에서는 자동으로 전파되고, 다른 task와는 분리된다.

## 사용처

- `session_id`
- `run_id`
- `turn_id`
- `tool_call_id`
- 도구 호출 카운터
- 토큰 사용량 추적

## 한 줄 정리

`ContextVar`는 **비동기 서버에서 요청별 컨텍스트 값을 안전하게 전파하는 Python 변수**이다.

## 관련

- [[async-await]]
- [[Observability]]
- [[Tool Observability Wrapping]]
- [[context manager]]
