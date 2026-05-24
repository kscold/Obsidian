- Single Agent = 하나의 [[LLM(Large Language Model)]]이 [[Tool Calling|도구 집합]]과 [[Memory|메모리]]를 가지고 사용자의 요청을 처음부터 끝까지 처리하는 가장 단순한 [[AI Agent|에이전트]] 토폴로지.
- 대부분의 production 에이전트는 사실 여기서 시작한다 — 멀티 에이전트는 단일 에이전트로 한계가 명확할 때 도입한다.

## 구조

```
사용자 → [Agent (LLM + Tools + Memory)] → 응답
              ↑─────────루프─────────┘
```

## 코드 예 (LangGraph 기반 ReAct 단일 에이전트)

```python
from langgraph.prebuilt import create_react_agent
from langchain_openai import ChatOpenAI

agent = create_react_agent(
    model=ChatOpenAI(model="gpt-4o-mini"),
    tools=[search_tool, calculator_tool],
)
agent.invoke({"messages": [("user", "오늘 환율로 100달러는 몇 원?")]})
```

## 장점

- **단순함** — 디버깅·관찰이 쉽다. 트레이스가 한 줄로 흐른다.
- **비용 효율** — LLM 호출이 한 명에 집중되어 토큰 낭비가 적다.
- **빠른 응답** — 에이전트 간 메시지 패싱 오버헤드 없음.

## 한계

- 도구 수가 늘어나면 LLM이 어느 도구를 골라야 할지 헷갈린다 (대략 도구 20개 이상부터 정확도 ↓).
- 한 에이전트의 컨텍스트 윈도우가 부족해지면 [[Multi Agent]]로 책임 분리가 필요.
- 도메인이 너무 넓으면 system prompt가 지나치게 길어진다.

## 언제 멀티 에이전트로 전환?

- 명확히 다른 역할(리서치 vs 글쓰기 vs 검증)이 필요할 때.
- 도구가 20+ 개를 넘기고 분리할 수 있을 때.
- 각 단계의 LLM 모델 선택(저렴 vs 강력)을 다르게 하고 싶을 때.

## 관련

- [[Multi Agent]] / [[Supervisor 패턴]] / [[Swarm 패턴]] / [[Hierarchical Agent]] — 확장형.
