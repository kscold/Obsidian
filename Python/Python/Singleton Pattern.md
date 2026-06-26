---
title: Singleton Pattern
created: 2026-06-26
tags:
  - python
  - pattern
---

# Singleton Pattern

Singleton Pattern은 어떤 객체를 프로세스 안에서 하나만 만들어 재사용하는 패턴이다.

## 왜 쓰나

- 임베딩 모델 로딩은 무겁다.
- BM25 인덱스는 한 번만 만들면 된다.
- 모델 레지스트리나 repository는 공통 상태를 가진다.

## Python식 예

```python
_singleton = None

def get_singleton():
    global _singleton
    if _singleton is None:
        _singleton = ExpensiveObject()
    return _singleton
```

동시성이 있으면 [[Double-Checked Locking]]을 함께 쓴다.

## 주의점

- 테스트 사이 상태가 남을 수 있다.
- 설정 변경을 반영하려면 reset 함수가 필요할 수 있다.
- 싱글톤이 너무 많으면 의존성이 숨는다.

## 한 줄 정리

Singleton Pattern은 **무거운 공용 객체를 한 번만 만들고 재사용하는 패턴**이다.

## 관련

- [[Lazy Initialization]]
- [[Double-Checked Locking]]
- [[Registry Pattern]]
