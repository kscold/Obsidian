- Intent Classification = 사용자의 자연어 입력을 **사전 정의된 의도(intent) 카테고리**로 분류해, 어떤 [[AI Agent|에이전트]] / 워크플로우 / 도구를 부를지 라우팅하는 단계.
- 챗봇/IVR 시절부터 있던 개념이지만, LLM 시대에는 **다층 파이프라인(빠른 규칙 → LLM)** 으로 발전한다.

## 왜 필요한가

- 한 에이전트가 모든 종류의 요청을 처리하려고 하면 system prompt가 비대해지고 정확도가 떨어진다.
- 미리 분류해서 **전문 에이전트(specialist)** 에게 보내면 각자가 자기 영역에만 집중할 수 있다.

## 3단계 파이프라인 (실무 표준)

```
입력
 ↓
[1단계] Rule / Regex   ── 명확한 키워드, 명령어 (~5ms)
 ↓ unmatched
[2단계] Embedding KNN  ── 의미 유사도, 동의어 (~30ms)
 ↓ low confidence
[3단계] LLM Classifier ── 모호한 자연어 (~500ms)
 ↓
intent + confidence → Router → 전문 에이전트/도구
```

- 빠른 단계에서 잡히는 게 90%면 비용·지연이 크게 줄어든다.

## 1) 규칙 기반

```python
def rule_classify(text: str) -> str | None:
    t = text.strip().lower()
    if t.startswith("/help"): return "help"
    if re.match(r"^주문\s+\d+", t): return "order_lookup"
    return None
```

## 2) 임베딩 KNN

```python
# 미리: 각 intent별 대표 예시들을 임베딩해 벡터 인덱스 적재
intents = {
    "refund": ["환불해줘", "돈 돌려줘", ...],
    "shipping": ["배송 언제 와", "택배 어디", ...],
}

def embed_classify(text: str, threshold=0.75) -> tuple[str, float] | None:
    v = embed(text)
    hit = vector_store.search(v, k=1)
    return (hit.intent, hit.score) if hit.score >= threshold else None
```

## 3) LLM 분류

```python
from pydantic import BaseModel
from typing import Literal

class IntentResult(BaseModel):
    intent: Literal["refund", "shipping", "general", "unknown"]
    confidence: float
    reason: str

prompt = f"""사용자 발화를 다음 카테고리 중 하나로 분류해라.
- refund: 환불 관련
- shipping: 배송 관련
- general: 일반 문의
- unknown: 분류 불가

발화: {text}"""

result = llm.with_structured_output(IntentResult).invoke(prompt)
```

## 신뢰도(confidence)와 fallback

- 낮은 confidence는 그대로 처리하지 말 것.
- 패턴:
  1. **clarify** — 사용자에게 되묻기 ("환불 말씀이신가요?")
  2. **escalate** — 사람 상담사로 전환
  3. **default agent** — 광범위 처리 가능한 fallback 에이전트

## Multi-intent (한 문장에 의도 둘)

- "환불하고 배송지도 바꿔줘" → refund + change_address 2개.
- Pydantic의 `list[Literal[...]]`로 다중 출력 강제.

## 운영 팁

- 분류 데이터셋을 git으로 관리 → 새 사례 추가 시 PR + [[Evaluation|회귀 평가]].
- production에서 "unknown" 비율을 모니터 — 분류 갱신 시그널.
- 카테고리는 5~15개가 적정 — 너무 많으면 LLM 정확도가 떨어진다 → [[Hierarchical Agent|계층 분류]]로 쪼개기.

## 관련

- [[Supervisor 패턴]] — 분류 결과로 전문 에이전트 라우팅.
- [[Routing Workflow]] · [[Agent vs Workflow]] — Workflow 패턴 중 하나.
- [[Structured Output]] · [[Pydantic]] — 신뢰성 있는 분류 출력.
