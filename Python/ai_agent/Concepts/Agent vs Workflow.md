- Anthropic의 ["Building Effective Agents"](https://www.anthropic.com/research/building-effective-agents) 정의를 따르면:
  - **Workflow** — 미리 정의된 코드 경로로 [[LLM(Large Language Model)]]과 도구를 호출하는 **결정론적 파이프라인**.
  - **Agent** — LLM이 **스스로 자신의 흐름과 도구 사용을 동적으로 결정**하고 작업 완료까지 루프를 도는 시스템.
- 둘 다 LLM을 쓰지만, **누가 흐름을 결정하느냐**가 본질적 차이다 — 코드(개발자)냐, LLM이냐.

## 결정 기준

| 상황 | 추천 |
|------|------|
| 입력·출력이 정해진 양식 (예: 메일 분류, 요약) | Workflow |
| 같은 입력엔 같은 동작을 원함 (재현성 중요) | Workflow |
| 작업 단계 수가 입력마다 다름 | Agent |
| 사용자의 모호한 자연어 목표를 수행 | Agent |
| 비용·지연·디버깅이 critical | Workflow |
| 자율성·유연성이 critical | Agent |

## Workflow 패턴 5가지 (결정론)

1. **Prompt Chaining** — LLM 호출을 순차로 이어붙임 (요약 → 번역 → 검수).
2. **Routing** — 입력을 분류해 가지를 친다 (질문 유형별 핸들러).
3. **Parallelization** — 같은 입력을 여러 모델·프롬프트에 동시에 (다수결 / 분할 정복).
4. **Orchestrator-Workers** — 마스터 LLM이 작업을 쪼개 워커 LLM에 분배.
5. **Evaluator-Optimizer** — 생성 LLM과 평가 LLM이 반복 개선.

## Agent의 본질

- 루프 안에서 **언제 멈출지 LLM이 스스로 정한다.**
- 다음에 어떤 [[Tool Calling|도구]]를 부를지, 어떤 순서로, 부를지 말지를 매 턴 결정.
- 그래서 [[LangGraph]] 같은 프레임워크는 Workflow(고정 그래프)와 Agent(동적 라우팅)를 한 시스템에서 섞을 수 있게 한다.

## 코드 비교

```python
# Workflow — 흐름이 코드에 고정됨
def summarize_then_translate(text: str) -> str:
    summary = llm.invoke(f"요약: {text}")
    translation = llm.invoke(f"영문 번역: {summary}")
    return translation

# Agent — LLM이 매 턴 다음 행동을 결정
agent = create_agent(
    llm=llm,
    tools=[summarize_tool, translate_tool, search_tool],
    system="사용자 목표를 달성할 때까지 도구를 골라 써라"
)
agent.invoke("이 문서를 영어로 요약해줘")
# → LLM이 알아서 summarize → translate 순으로 호출
```

## 실무 권장

- 대부분의 production 시스템은 **Workflow가 더 적합**하다. Agent는 마지막 카드.
- Agent를 쓰더라도 그 안의 각 노드는 가급적 Workflow로 짜는 게 비용·신뢰성 면에서 유리.
