- Evaluator-Optimizer = **생성 LLM(Optimizer)** 과 **평가 LLM(Evaluator)** 을 짝지어, 평가 점수가 임계치를 넘을 때까지 반복 개선하는 [[Agent vs Workflow|워크플로우 패턴]].
- Anthropic의 5가지 workflow 패턴 중 하나. 코드·번역·글쓰기처럼 **품질이 모호하고 반복 개선이 가능한** 작업에 강하다.

## 구조

```
입력 → Optimizer (draft) → Evaluator (score, critique)
                              ↓
                  score >= threshold ? → 출력
                              │ else
                  critique + draft 를 Optimizer로 되먹임 → 반복
```

## 코드

```python
def evaluator_optimizer_loop(task: str, threshold: float = 0.8, max_iters: int = 4):
    feedback = None
    for _ in range(max_iters):
        prompt = task if not feedback else f"{task}\n이전 피드백: {feedback}"
        draft = optimizer.invoke(prompt)

        verdict = evaluator.invoke({"task": task, "draft": draft})
        if verdict.score >= threshold:
            return draft
        feedback = verdict.critique
    return draft  # 최선의 시도
```

## [[Reflection]]과의 차이

| | Reflection | Evaluator-Optimizer |
|--|-----------|---------------------|
| 평가자 | 같은 LLM 자기 비평 | 별도 [[LLM-as-Judge|평가자 LLM]] (보통 더 강함) |
| 종료 조건 | 비결정적 | 점수 임계치 |
| 적합 영역 | 빠른 자기교정 | 품질이 객관적으로 평가되는 작업 |

## 효과적인 시나리오

- **번역** — Evaluator가 원문 충실도·자연스러움 채점.
- **코드** — Evaluator가 컴파일·테스트 통과 여부.
- **카피라이팅** — 톤·길이·금기어 검토.

## 한계

- 매 반복이 LLM 호출 → 비용·지연 N배.
- Evaluator가 잘못된 기준을 학습하면 잘못된 방향으로 수렴.
- 멈춰야 할 때 못 멈추면 무한 회귀 → `max_iters` 필수.

## 관련

- [[Reflection]] · [[Reflexion]] — 자기 평가 메커니즘.
- [[LLM-as-Judge]] — Evaluator의 핵심 메커니즘.
- [[Plan-and-Execute]] — Replanner도 일종의 Evaluator-Optimizer.
- [[Agent vs Workflow]] — 같은 계열의 다른 패턴(Prompt chaining, Routing, Parallelization, Orchestrator-Workers).
