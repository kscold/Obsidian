- Evaluation = [[AI Agent|에이전트]]가 **얼마나 잘 동작하는지 체계적으로 측정**하는 절차. 에이전트는 LLM·도구·프롬프트의 비결정성 때문에 "테스트는 됐는데 운영에선 안 된다"가 일상이라, 평가 루프 없이는 운영이 무너진다.
- 평가 루프는 보통 3계층으로 나눈다: **Inner Loop**(개발자) → **Outer Loop**(릴리즈) → **Production Loop**(운영).

## 왜 어려운가 — 4가지 도전

| # | 어려움 | 의미 |
|---|--------|------|
| ① | 비결정성 | 같은 입력에 다른 출력. 단위 테스트로 못 잡음 |
| ② | 잘못된 도구/파라미터 | 도구는 맞는데 인자가 틀린 케이스가 많음 |
| ③ | "더 나음" 측정 곤란 | 프롬프트 한 글자 차이도 결과를 바꿈 |
| ④ | 자동 회귀 | 모델·프롬프트·도구 교체 때마다 회귀 가능 |

## 3가지 평가 루프

### Inner Loop — 개발자 노트북

- 개발 중 즉시 피드백.
- **[[ReAct 패턴]] / [[Tool Calling]] trajectory 확인** + 빠른 회귀 테스트.
- 도구: `pytest`, [[LangChain]] LangSmith 로컬 모드, Strands Evaluations.

### Outer Loop — 릴리즈 전 회귀

- 데이터셋(수십~수백 시나리오)에 대해 일괄 평가.
- 변경 전/후 점수를 비교하고 **A/B 테스트**.
- 도구: AgentCore Evaluation, RAGAS, DeepEval, LangSmith eval.

### Production Loop — 운영 모니터링

- 실시간 사용자 트래픽 샘플링 → 자동 평가.
- 회귀가 감지되면 알림 / 롤백.
- 도구: [[Observability]] 시스템 + 자동 evaluator.

## 평가 방식 3가지

### 1. Code-based Evaluator

```python
def eval_exact_match(answer, expected):
    return 1.0 if answer.strip() == expected.strip() else 0.0
```

- 정답이 명확한 경우 (분류, 수식 결과).
- 빠르고 결정론적.

### 2. [[LLM-as-Judge]]

```python
prompt = f"질문: {q}\n답변: {a}\n참고: {ref}\n0~10으로 평가하고 이유를 적어라."
score = llm.invoke(prompt)
```

- 자유 형식 답변에 흔히 씀. 빠르고 유연하지만 편향·일관성 문제.

### 3. Human Evaluation

- 가장 정확하지만 비싸고 느림.
- Inner Loop의 골든 데이터 수집용으로 주로 사용.

## 측정 차원

| 차원 | 의미 | 예시 |
|------|------|------|
| **Faithfulness** | 답변이 컨텍스트에 충실한가 | RAG 환각 감지 |
| **Relevancy** | 질문과 관련 있는가 | 주제 이탈 검출 |
| **Correctness** | 사실관계가 맞는가 | ground truth와 비교 |
| **Tool accuracy** | 올바른 도구·인자를 골랐나 | trajectory 비교 |
| **Latency / Cost** | 응답 속도·토큰 비용 | 운영 KPI |
| **Safety** | 유해·민감 응답 여부 | [[Guardrails]] |

## Ground Truth 3형태

- **Reference Answer** — "정답"이 텍스트로 있음.
- **Reference Trajectory** — 어떤 도구 순서로 풀어야 했는지.
- **Rubric** — 채점 기준만 자연어로.

## User Simulator

- 실제 사용자 트래픽이 부족할 때, LLM이 다양한 페르소나로 가상의 질의를 생성 → 에이전트 자동 부하 테스트.

## 권장 운영 팁

- **회귀 데이터셋을 git으로 관리** — 시나리오와 기대 결과를 코드처럼 PR로.
- 변경 시 항상 **세 모델로 평가**(현재 모델, 이전 모델, 후보 모델) — 자동 회귀 방지.
- 평가 비용도 KPI — 평가 LLM은 production 모델보다 강한 것을 쓰되 호출 횟수는 제한.

## 관련

- [[LLM-as-Judge]] — 평가자 LLM.
- [[Observability]] — Production Loop의 인프라.
- [[Guardrails]] — Safety 평가와 보완.
- [[Trajectory]] — Tool accuracy 평가 단위.
