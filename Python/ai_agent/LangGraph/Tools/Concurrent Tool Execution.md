---
title: Concurrent Tool Execution
created: 2026-07-15
tags:
  - ai-agent
  - tool-calling
  - concurrency
  - reliability
---

# Concurrent Tool Execution

Concurrent Tool Execution은 한 LLM 응답이 여러 tool call을 낼 때, 어떤 호출을 병렬로 실행하고 어떤 호출을 순서대로 실행할지 정하는 설계다. 빠르게 실행하는 것보다 **공유 상태를 깨뜨리지 않는 실행 모델을 먼저 선택**해야 한다.

## 병렬과 직렬의 기준

| 도구 성격 | 예 | 실행 방식 |
|---|---|---|
| 독립 읽기 | 문서 검색, 날씨 조회, 계산 | 병렬 가능 |
| 독립 생성 | 서로 다른 초안 작성, 후보 점수화 | 병렬 가능, 결과를 reducer로 병합 |
| 같은 자원 쓰기 | 같은 주문, 파일, 캔버스, 레코드 수정 | 직렬 또는 트랜잭션 |
| 선후 의존 쓰기 | 생성 후 연결, 조회 후 수정 | 명시적인 순서 보장 |
| 고위험 쓰기 | 배포, 결제, 삭제 | [[Human-in-the-loop\|승인]] 후 보수적으로 실행 |

## 안전한 병렬 실행

```python
import asyncio

async def collect_evidence(query: str):
    tasks = [
        search_docs(query),
        search_database(query),
        search_web(query),
    ]
    return await asyncio.gather(*tasks)
```

이 패턴은 각 작업이 다른 상태를 쓰지 않고, 실패 처리와 병합 기준이 정해져 있을 때만 적합하다. API rate limit과 자원 사용량을 제어하려면 `Semaphore`로 동시성 상한을 둔다.

```python
limit = asyncio.Semaphore(4)

async def bounded_call(coro):
    async with limit:
        return await coro
```

## 직렬화가 필요한 경우

여러 도구가 하나의 mutable state를 읽고 수정하면 병렬 호출은 lost update를 만든다. 예를 들어 `add_item`과 `connect_item`은 첫 호출의 결과가 두 번째 호출의 입력이므로 순서를 보장해야 한다.

```mermaid
flowchart LR
    Read[현재 snapshot 읽기] --> Mutate1[첫 변경]
    Mutate1 --> Commit1[저장 / version 증가]
    Commit1 --> Mutate2[다음 변경]
```

해결 방법은 다음 중 하나다.

- tool call을 모델이 낸 순서대로 직렬 실행한다.
- version number 또는 compare-and-swap으로 충돌을 감지하고 재시도한다.
- DB transaction 안에서 여러 변경을 원자적으로 처리한다.
- 모든 도구가 직접 state를 쓰지 않고 action 목록만 반환한 뒤, 한 노드가 결정적으로 적용한다.

## 결과 병합과 오류

- 병렬 읽기 결과는 출처, 시간, 실패 여부를 유지한 채 병합한다.
- 하나의 보조 검색 실패가 전체 요청을 막지 않아도 되는지 [[Fail-soft]] 정책으로 정한다.
- 모든 호출의 오류를 무시하면 모델이 빈 결과를 "없음"으로 오해할 수 있다. 실패와 빈 결과를 구분해 전달한다.
- 도구 호출 횟수, 병렬도, timeout, 취소를 [[Observability|관측]]한다.

## 관련

- [[LangGraph ToolNode]]
- [[LangGraph Send]]
- [[LangGraph State Reducer]]
- [[Tool Execution Policy]]
- [[async-await]]
- [[Async Resource Lifecycle]]

## 공식 문서

- [LangChain Tools: 병렬 실행·state update](https://docs.langchain.com/oss/python/langchain/tools)
- [LangGraph Graph API: Send와 reducer](https://docs.langchain.com/oss/python/langgraph/use-graph-api)
