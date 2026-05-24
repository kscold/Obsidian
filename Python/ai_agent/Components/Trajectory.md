- Trajectory = [[AI Agent|에이전트]]가 목표를 달성하기까지 **거친 모든 사고·도구 호출·관찰의 시퀀스**. "이 답이 어떻게 나왔는가"를 시간 순으로 정렬한 기록.
- [[Evaluation|평가]]·디버깅·재현·[[Reflexion|학습]]의 단위가 된다 — 단순 input/output보다 훨씬 정보가 풍부.

## 표준 형태

```json
{
  "task": "오늘 환율로 100달러는 몇 원?",
  "steps": [
    {"type": "thought",     "text": "환율 조회가 필요하다"},
    {"type": "tool_call",   "name": "get_fx", "args": {"pair": "USD/KRW"}},
    {"type": "observation", "text": "1380.5"},
    {"type": "tool_call",   "name": "multiply", "args": {"a": 100, "b": 1380.5}},
    {"type": "observation", "text": "138050"},
    {"type": "final",       "text": "약 138,050원입니다."}
  ],
  "metrics": {"latency_ms": 2300, "tokens_in": 412, "tokens_out": 88}
}
```

## 왜 보존해야 하나

1. **사후 디버깅** — 같은 입력에 다른 출력이 나왔을 때 분기점을 찾는 유일한 단서.
2. **회귀 평가** — `expected_trajectory`와 실제를 비교해 "도구를 올바르게 골랐나" 측정.
3. **자기 학습** — [[Reflexion]]은 실패한 trajectory를 다음 시도 메모리로 사용.
4. **사용자에게 근거 제공** — "왜 이렇게 답했나"를 보여줄 수 있다.

## 수집 방법

- [[Observability]] 시스템(OpenTelemetry, LangSmith)에서 트레이스로 자동 수집.
- 또는 그래프 상태에 명시적으로 push.

```python
class State(TypedDict):
    messages: list
    trajectory: list   # 노드마다 append
```

## Trajectory-based Evaluation

```python
def eval_trajectory(actual, expected) -> dict:
    return {
        "tool_correctness": tool_match_rate(actual.tools, expected.tools),
        "step_efficiency": expected.steps_count / actual.steps_count,
        "final_match": llm_judge(actual.final, expected.final),
    }
```

- 최종 답뿐 아니라 **경로의 효율성**도 평가 가능.

## 압축

- trajectory가 길어지면 [[Memory|단기 메모리]]가 폭주.
- 요약·청크 단위로 압축해 다음 턴 컨텍스트로 넣는다.

## 사용자 노출

- 일부 에이전트(특히 Cursor·Devin류)는 trajectory를 UI에 그대로 펼친다 → 신뢰감.
- 단, 내부 사고가 그대로 보여 보안·UX 측면에서 조정 필요.

## 관련

- [[ReAct 패턴]] — trajectory의 텍스트 표현.
- [[Reflexion]] — 실패 trajectory를 다음 시도에 활용.
- [[Evaluation]] · [[LLM-as-Judge]] — trajectory를 평가.
- [[Observability]] — 자동 수집 인프라.
