---
title: Annotated
created: 2026-06-26
tags:
  - python
  - typing
---

# Annotated

`Annotated`는 타입에 추가 메타데이터를 붙이는 typing 도구이다.

기본 타입은 유지하면서, 프레임워크가 읽을 부가 정보를 함께 전달할 때 쓴다.

## 예

```python
from typing import Annotated
import operator

actions: Annotated[list[dict], operator.add]
```

이 예에서 타입은 `list[dict]`이고, `operator.add`는 LangGraph가 state 값을 병합할 때 쓰는 reducer 메타데이터이다.

## LangGraph에서

```python
messages: Annotated[list[BaseMessage], add_messages_reducer]
```

노드가 `messages`를 반환할 때 기존 값을 덮어쓰는 대신 reducer로 합친다.

## 한 줄 정리

`Annotated`는 **타입에 프레임워크가 읽을 실행 메타데이터를 붙이는 방법**이다.

## 관련

- [[TypedDict]]
- [[LangGraph State Reducer]]
- [[typing 모듈]]
