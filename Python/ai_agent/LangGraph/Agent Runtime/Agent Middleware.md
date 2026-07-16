---
title: Agent Middleware
created: 2026-07-15
tags:
  - ai-agent
  - langchain
  - langgraph
  - middleware
---

# Agent Middleware

Agent Middleware는 agent loop의 핵심 판단 코드를 바꾸지 않고, 모델 호출과 도구 실행의 **정해진 생명 주기 지점에 공통 정책을 삽입하는 확장 패턴**이다. LangChain의 middleware는 create_agent가 만드는 LangGraph 실행 안에서 동작하므로, 더 큰 StateGraph의 노드나 subgraph로도 합성할 수 있다.

## 어디에 끼어드는가

~~~mermaid
flowchart LR
    Start["agent 시작"] --> BeforeAgent["before agent"]
    BeforeAgent --> BeforeModel["before model"]
    BeforeModel --> Model["model 호출"]
    Model --> AfterModel["after model"]
    AfterModel --> Tool["tool 호출"]
    Tool --> AfterTool["tool 결과 처리"]
    AfterTool --> Finish["agent 종료"]
~~~

| 지점 | 적합한 책임 |
|---|---|
| agent 시작 전후 | 요청 ID, 사용자 권한, 전체 예산, 최종 감사 |
| model 호출 전후 | 컨텍스트 주입·요약, 모델 routing, 출력 검사 |
| model 호출 wrapper | retry, fallback, token 측정, trace |
| tool 호출 wrapper | 인자 검증, 승인, timeout, 결과 정제 |

고정된 훅에서 반복되는 정책을 처리하는 것이 middleware의 강점이다. 입력 분류 뒤 서로 다른 subgraph로 갈라지거나, 병렬 fan-out을 만드는 **토폴로지 자체의 변화**는 [[LangGraph StateGraph|명시적인 LangGraph 노드와 edge]]로 표현하는 편이 더 읽기 쉽다.

## LangChain의 기본 사용

~~~python
from langchain.agents import create_agent
from langchain.agents.middleware import (
    HumanInTheLoopMiddleware,
    SummarizationMiddleware,
)

agent = create_agent(
    model="your-model",
    tools=[search_docs, send_email],
    middleware=[
        SummarizationMiddleware(...),
        HumanInTheLoopMiddleware(
            interrupt_on={"send_email": True}
        ),
    ],
)
~~~

이 구성에서 대화 요약은 context window를 관리하고, 이메일 전송은 실제 tool 실행 전에 사람 승인을 요구한다. middleware는 도구의 이름과 실행 상태를 정확히 알아야 하므로, tool 이름과 정책 식별자를 안정적으로 유지해야 한다.

## 넣기 좋은 것과 나쁜 것

**넣기 좋은 것**

- [[Context Engineering|컨텍스트]] 축약·주입
- [[LLM Routing|모델 선택]]과 fallback
- [[Guardrails|PII·정책 검사]]
- [[Tool Execution Policy|도구 권한·승인·예산 검사]]
- trace, token, latency, 오류 분류

**명시적 노드가 더 좋은 것**

- 업무 규칙에 따른 분기와 상태 전이
- 서로 다른 Agent로의 supervisor routing
- 병렬 작업의 fan-out과 deterministic merge
- 재시도 가능한 외부 효과의 단계적 실행

## 구현 원칙

1. middleware는 가능한 한 **idempotent**하게 만든다. 재실행돼도 로그 중복 외의 외부 효과가 없어야 한다.
2. 상태 변경은 명시적인 schema와 reducer를 통해 남긴다. 숨은 전역 상태에 결과를 저장하지 않는다.
3. tool 인자나 모델 출력을 바꾸면 원본·변환 사유·정책 버전을 trace에 남긴다.
4. middleware 자체가 실패할 때 allow, deny, fallback 중 무엇을 할지 정책으로 정한다.
5. 다른 middleware와의 순서가 결과를 바꾸므로, 보안 검사는 우회되지 않는 앞쪽에 둔다.

## 관련

- [[LangGraph create_react_agent]]
- [[LangGraph ToolNode]]
- [[Guardrails]]
- [[Human-in-the-loop]]
- [[Tool Execution Policy]]
- [[Context Engineering]]
- [[Observability]]

## 공식 문서

- [LangChain Middleware 개요](https://docs.langchain.com/oss/python/langchain/middleware/overview)
- [LangChain Custom Middleware](https://docs.langchain.com/oss/python/langchain/middleware/custom)
