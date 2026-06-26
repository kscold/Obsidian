---
title: Double-Checked Locking
created: 2026-06-26
tags:
  - python
  - concurrency
  - pattern
---

# Double-Checked Locking

Double-Checked Locking은 싱글톤 객체를 만들 때 lock 비용을 줄이면서 thread-safe하게 초기화하는 패턴이다.

## 구조

```python
if obj is None:
    with lock:
        if obj is None:
            obj = create()
return obj
```

첫 번째 체크는 이미 초기화된 뒤 lock을 잡지 않기 위한 성능 최적화이고, 두 번째 체크는 여러 스레드가 동시에 들어왔을 때 중복 생성을 막기 위한 안전장치이다.

## 사용처

- 임베딩 모델 싱글톤
- BM25 인덱스 싱글톤
- 카탈로그 repository 싱글톤

## 주의점

- lock 안에서 다시 확인하지 않으면 두 스레드가 객체를 두 번 만들 수 있다.
- 초기화가 실패했을 때 partial object가 남지 않도록 조심해야 한다.

## 한 줄 정리

Double-Checked Locking은 **무거운 싱글톤 객체를 한 번만 만들면서 매 호출마다 lock을 잡지 않게 하는 패턴**이다.

## 관련

- [[Singleton Pattern]]
- [[Lazy Initialization]]
- [[BGE-M3]]
