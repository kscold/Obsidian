---
title: Factory Pattern
created: 2026-06-26
tags:
  - python
  - pattern
---

# Factory Pattern

Factory Pattern은 객체 생성 로직을 호출부에서 분리해 별도 함수나 클래스로 숨기는 패턴이다.

## 예

```python
def get_vectorstore(collection_name):
    client = QdrantClient(...)
    return QdrantVectorStore(client=client, collection_name=collection_name)
```

호출자는 Qdrant 연결, 컬렉션 생성, embedding 주입을 몰라도 된다.

## AI Agent에서의 사용

- LLM 생성: provider에 따라 OpenAI/Ollama 인스턴스 생성
- VectorStore 생성: Qdrant 컬렉션 보장 후 생성
- Retriever 생성: search kwargs를 반영해 반환

## 장점

- 생성 규칙을 한 곳에 모은다.
- provider 차이를 감춘다.
- 테스트에서 fake factory를 주입하기 쉽다.

## 한 줄 정리

Factory Pattern은 **복잡한 객체 생성 과정을 한 곳에 모아 호출부가 사용 의도만 표현하게 하는 패턴**이다.

## 관련

- [[LLM Provider 추상화]]
- [[Qdrant]]
- [[Registry Pattern]]
