---
title: Mixin
created: 2026-06-26
tags:
  - python
  - oop
  - pattern
---

# Mixin

Mixin은 클래스에 특정 기능 묶음을 조합하기 위해 사용하는 작은 부모 클래스이다.

핵심 객체 하나가 너무 커질 때, 기능별 mixin으로 나누고 최종 클래스가 다중 상속으로 조합한다.

## 예

```python
class BlockCatalogRAGService(
    RAGIndexBuilderMixin,
    RAGRetrievalMixin,
    RAGFormatterMixin,
):
    ...
```

## 언제 쓰나

- index build 기능
- retrieval 기능
- formatting 기능
- 공통 helper 묶음

## 주의점

- mixin은 독립 실행 객체가 아니라 조합용이다.
- 같은 이름의 메서드가 충돌하지 않게 해야 한다.
- 상속 순서가 Method Resolution Order에 영향을 준다.

## 한 줄 정리

Mixin은 **큰 클래스를 기능 단위로 나누고 다중 상속으로 조합하는 Python OOP 패턴**이다.

## 관련

- [[Factory Pattern]]
- [[Hybrid RAG Search]]
