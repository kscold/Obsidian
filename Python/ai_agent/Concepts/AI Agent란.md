- [[AI Agent]]는 [[LLM(Large Language Model)]]이 **단순히 텍스트를 생성하는 것을 넘어**, 외부 [[Tool Calling|도구]]를 호출하고 그 결과를 다시 입력으로 받아 추론을 이어가는 **자율 실행 루프**를 갖춘 시스템이다.
- 사람이 한 줄씩 명령하지 않아도, "비행기 예약해줘"처럼 모호한 목표를 받아서 → 검색 → 후보 비교 → 결제 도구 호출 같은 다단계 작업을 스스로 수행한다.
- 핵심은 **Loop**: `관찰(Observation) → 추론(Reasoning) → 행동(Action) → 관찰` 을 종료 조건이 만족될 때까지 반복한다.

## Agent의 4가지 핵심 능력

1. **Reasoning(추론)** — 현재 상태에서 다음에 무엇을 해야 할지 결정. [[LLM(Large Language Model)]]이 담당.
2. **Tool Use(도구 사용)** — 검색, DB 조회, 코드 실행, API 호출 등 외부 능력을 호출. [[Tool Calling]] 참조.
3. **Memory(기억)** — 직전 대화(단기)와 장기적 사실(장기)을 유지. [[Memory]] 참조.
4. **Planning(계획)** — 복잡한 목표를 하위 작업으로 분해. [[Planning]] 참조.

## 일반 LLM 호출과의 차이

```python
# 일반 LLM 호출 — 단발성, 입력 → 출력으로 끝
response = llm.invoke("오늘 서울 날씨 알려줘")
# → LLM은 학습 시점 이후의 실시간 정보를 모름

# Agent 호출 — 도구를 통해 외부와 상호작용
agent = create_agent(llm, tools=[weather_tool, search_tool])
response = agent.invoke("오늘 서울 날씨 알려줘")
# → LLM이 weather_tool을 골라 호출 → 결과 받아 응답 생성
```

## Agent의 실행 루프 (의사 코드)

```python
def agent_loop(goal: str, tools: list, max_iters: int = 10):
    history = [goal]
    for _ in range(max_iters):
        # 1. LLM이 다음 행동 결정
        action = llm.decide_next_action(history, tools)

        # 2. 종료 조건 확인
        if action.is_final_answer:
            return action.answer

        # 3. 도구 실행
        observation = tools[action.name].run(action.args)

        # 4. 결과를 history에 반영 → 다음 루프
        history.append((action, observation))
```

## 언제 Agent가 필요한가

- **필요한 경우**: 작업이 동적이라 미리 코드로 흐름을 박을 수 없을 때 (사용자의 모호한 요청, 다단계 리서치, 자동화 워크플로우).
- **불필요한 경우**: 흐름이 고정된 단순 [[Agent vs Workflow|워크플로우]]는 Agent보다 결정론적 파이프라인이 낫다 — 토큰 비용·지연·예측 불가능성을 피할 수 있다.

## 한계와 주의점

- LLM의 환각(hallucination)이 그대로 행동으로 이어지면 잘못된 도구 호출로 번진다.
- 루프가 무한히 도는 것을 막기 위해 항상 `max_iters` 같은 안전장치가 필요하다.
- 도구가 외부 시스템을 변경할 때는 [[Human-in-the-loop]] 승인 단계를 두는 것이 안전하다.
