---
title: invoke
created: 2026-06-22
tags:
  - python
  - ai-agent
  - langchain
  - langgraph
---

# invoke

## 정의

`invoke()`는 LangChain과 LangGraph에서 자주 쓰는 실행 메서드이다.

뜻은 다음과 같다.

```text
이 객체에 입력값을 넣고 실제로 실행해서 결과를 받아와라.
```

## 예시

### LLM 실행

```python
response = llm.invoke(prompt)
```

LLM에게 prompt를 넣고 응답을 받는다.

### Chain 실행

```python
result = chain.invoke({"text": "Hello, World!"})
```

chain 전체를 실행한다.

프롬프트 안의 `{text}` 자리에 `"Hello, World!"`가 주입된다.

### Tool Binding된 LLM 실행

```python
response = llm_with_tools.invoke(current_messages)
```

도구 목록을 알고 있는 LLM에게 현재 메시지를 넣는다.

이때 LLM은 다음 중 하나를 선택한다.

- 도구 없이 바로 답변
- tool call 생성

### Graph 실행

```python
result = graph.invoke({"messages": "감기 걸렸을 때 뭘 먹어야 해?"})
```

LangGraph 전체를 초기 State로 실행한다.

## 준비와 실행의 차이

```python
llm_with_tools = llm.bind_tools(mytools)
```

위 코드는 실행이 아니라 준비이다.

```python
response = llm_with_tools.invoke(current_messages)
```

위 코드가 실제 실행이다.

## 한 줄 정리

> `invoke()`는 LangChain/LangGraph 객체에 입력을 넣고 실제 작업을 실행하는 메서드이다.

관련:

- [[LangGraph 문법 치트시트]]
- [[LLM Tool Selection]]

