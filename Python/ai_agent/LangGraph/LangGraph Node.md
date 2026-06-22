---
title: LangGraph Node
created: 2026-06-22
tags:
  - ai-agent
  - langgraph
  - node
  - workflow
---

# LangGraph Node

## 정의

`Node`는 LangGraph 그래프 안에서 실행되는 하나의 작업 단계이다.

보통 Python 함수로 만들고, `builder.add_node()`로 그래프에 등록한다.

```python
builder.add_node("retrieve", retrieve)
builder.add_node("generate", generate)
```

## Node 함수의 기본 형태

```python
def node_name(state: State):
    # state를 읽고
    # 필요한 작업을 수행한 뒤
    return {"some_key": some_value}
```

입력은 보통 [[LangGraph State]]이고, 출력은 State에 병합할 딕셔너리이다.

## 예시: retrieve 노드

```python
def retrieve(state: State):
    question = state["question"]
    docs = [
        "생강차: 몸을 따뜻하게 하고 염증을 가라앉혀 목 통증 완화에 좋다.",
        "닭고기 수프: 수분과 단백질을 보충하고 코막힘을 풀어주는 데 도움이 된다.",
    ]
    return {"context": docs}
```

`retrieve`는 답변에 필요한 참고 문서를 준비하는 노드이다.

아래 예시는 실제 검색을 하지 않고, 문서를 하드코딩해서 반환하는 가장 단순한 형태이다.

## 예시: generate 노드

```python
def generate(state: State):
    context = "\n".join(state["context"])

    prompt = f"""다음 문맥을 참고해 질문에 답하세요.

문맥:
{context}

질문: {state["question"]}

답변:"""

    response = llm.invoke(prompt)
    return {"answer": response.content}
```

`generate`는 `context`와 `question`을 프롬프트로 합쳐 LLM에게 전달하고, 결과를 `answer`에 저장한다.

## Node와 일반 함수의 차이

함수 자체는 일반 Python 함수이다.

하지만 LangGraph에 등록되면 그래프 실행 단위가 된다.

```python
builder.add_node("retrieve", retrieve)
```

이 순간부터 `retrieve`는 단순 함수가 아니라 그래프의 한 단계로 동작한다.

## Node와 Tool의 차이

`retrieve` 같은 Node는 그래프 설계자가 실행 순서를 정한다.

반면 `@tool`이 붙은 함수는 LLM이 필요할 때 선택해서 호출한다.

자세한 비교:

- [[Workflow Node vs Tool]]
- [[LangChain @tool]]
- [[Tool Calling]]

## 한 줄 정리

> LangGraph Node는 State를 입력받아 작업을 수행하고, State에 병합될 결과를 반환하는 워크플로우 단계이다.
