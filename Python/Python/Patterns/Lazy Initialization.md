---
title: Lazy Initialization
created: 2026-06-26
tags:
  - python
  - pattern
---

# Lazy Initialization

Lazy Initialization은 객체가 실제로 필요해질 때까지 생성을 미루는 패턴이다.

AI Agent에서는 Qdrant 연결, 임베딩 모델, LangGraph agent, vectorstore처럼 무거운 객체가 많기 때문에 import 시점에 전부 만들면 부팅이 느려지고 순환 import도 생기기 쉽다.

## 예

```python
class LazyVectorStore:
    def __init__(self):
        self._store = None

    def _get(self):
        if self._store is None:
            self._store = create_store()
        return self._store
```

## 장점

- 부팅이 빨라진다.
- 사용하지 않는 기능의 의존성을 로드하지 않는다.
- 순환 import 위험을 줄인다.
- 테스트에서 무거운 외부 연결을 피하기 쉽다.

## 한 줄 정리

Lazy Initialization은 **필요한 순간까지 무거운 객체 생성을 미뤄 비용과 의존성 문제를 줄이는 패턴**이다.

## 관련

- [[Qdrant]]
- [[__getattr__ Lazy Import]]
- [[Singleton Pattern]]
- [[Double-Checked Locking]]
