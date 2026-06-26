---
title: functools.wraps
created: 2026-06-26
tags:
  - python
  - decorator
---

# functools.wraps

`functools.wraps`는 wrapper 함수가 원래 함수의 이름, docstring, metadata를 유지하게 해주는 데코레이터이다.

## 예

```python
import functools

def make_wrapper(orig):
    @functools.wraps(orig)
    def wrapper(*args, **kwargs):
        return orig(*args, **kwargs)
    return wrapper
```

## 왜 필요한가

wrapper를 만들면 함수 이름이 전부 `wrapper`로 보일 수 있다. 로그, 디버깅, introspection에서 원래 함수 정보가 사라진다.

`wraps`를 붙이면 원래 함수의 `__name__`, `__doc__` 등을 보존한다.

## 사용처

- [[데코레이터(Decorator)]]
- 도구 호출 wrapper
- 로깅/측정 wrapper
- 권한 검사 wrapper

## 한 줄 정리

`functools.wraps`는 **함수를 감쌀 때 원래 함수의 정체성을 보존해주는 데코레이터 보조 도구**이다.

## 관련

- [[데코레이터(Decorator)]]
- [[Tool Observability Wrapping]]
