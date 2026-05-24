- Planning = [[AI Agent|에이전트]]가 **복잡한 목표를 더 작은 하위 작업(sub-task)으로 분해**하고, 그 순서를 정해 수행하는 능력.
- 모든 에이전트가 명시적 planner를 갖진 않는다 — [[ReAct 패턴]]처럼 스텝마다 즉흥적으로 결정하는 방식과, [[Plan-and-Execute]]처럼 미리 계획을 세우는 방식이 있다.

## 명시적 Planner 분리

```python
class Plan(BaseModel):
    steps: list[str]

planner = llm.with_structured_output(Plan)
plan = planner.invoke(f"목표: {goal}\n단계별 계획을 세워라")
# plan.steps == ["사용자 정보 조회", "최근 주문 가져오기", "환불 처리"]
```

- Pydantic으로 구조화된 출력을 강제하면 후속 실행이 안정적.

## 대표 전략

- **Decomposition** — 목표를 하위 작업으로 쪼갠다 (위 예시).
- **Tree of Thoughts** — 여러 후보 분기를 생성하고 평가해 최선을 고른다.
- **Self-Ask** — "이 질문을 답하려면 무엇을 먼저 알아야 하나?"를 LLM이 스스로 묻는다.
- **HTN (Hierarchical Task Network)** — 작업을 트리로 분해.

## Replanning이 중요

- 처음 세운 계획이 실패하거나 외부 환경이 바뀌면 그대로 진행하면 안 됨.
- 매 단계 후 "계획이 여전히 유효한가?"를 LLM에 묻고 갱신.

```python
def step_loop(plan, goal, history):
    while plan:
        step = plan.pop(0)
        result = executor.run(step)
        history.append((step, result))
        decision = replanner.invoke({"goal": goal, "history": history, "remaining": plan})
        if decision.done: return decision.answer
        plan = decision.new_plan
```

## 한계

- LLM의 계획 능력은 도메인 지식에 의존 — 모르는 도메인은 망상에 가까운 계획.
- 계획이 길수록 실행 비용이 곱해진다.
- 너무 잘게 쪼개면 단계 간 정보 손실이 커진다.

## 관련

- [[ReAct 패턴]] — 사전 계획 없이 step-by-step.
- [[Plan-and-Execute]] — Planner + Executor 분리.
- [[Reflexion]] — 실패에서 학습해 다음 계획 보정.
