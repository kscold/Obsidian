- LLM-as-Judge = **LLM에게 다른 LLM의 출력(또는 에이전트의 행동)을 채점하게 하는** [[Evaluation|평가]] 기법.
- 자유 형식 답변·다단계 trajectory처럼 코드로 채점하기 어려운 대상에 거의 유일한 자동화 수단이다. 단, **편향과 일관성 문제**를 알고 써야 한다.

## 가장 단순한 형태

```python
prompt = f"""
다음 답변을 0~10으로 채점하고 이유를 적어라.

질문: {question}
답변: {answer}
참고 자료: {reference}

출력 형식:
score: <0-10>
reason: <이유>
"""
score = llm.invoke(prompt)
```

## 구조화된 평가 (권장)

```python
from pydantic import BaseModel
from langchain_openai import ChatOpenAI

class Judgement(BaseModel):
    faithfulness: int    # 0~10, 컨텍스트 충실도
    relevancy: int
    completeness: int
    reason: str

judge = ChatOpenAI(model="gpt-4o").with_structured_output(Judgement)
verdict = judge.invoke(rubric_prompt)
```

- [[Pydantic]] 스키마를 강제해 파싱 실패를 막는다.

## 흔한 편향 4종

1. **Position bias** — pairwise 비교 시 먼저 보여준 쪽을 선호. → 좌우 위치를 swap 해서 두 번 평가.
2. **Length bias** — 더 긴 답변을 선호. → 길이를 명시적으로 평가 대상에서 제외하라고 프롬프트.
3. **Self-enhancement** — 같은 모델 패밀리(예: GPT-4 평가 GPT-4 답변)에 우호적. → **다른 패밀리 모델**을 평가자로.
4. **Style bias** — 형식 깔끔한 답변을 선호. → rubric을 사실성·정확성 위주로.

## Pairwise vs Pointwise

- **Pointwise** — 한 답변에 점수. 절대 평가.
- **Pairwise** — 두 답변을 비교해 선호. 상대 평가, 일관성이 더 좋음.
- 운영에서는 pairwise로 모델/프롬프트를 비교, pointwise로 절대 품질 모니터링.

## Reference-based vs Reference-free

| | Reference-based | Reference-free |
|--|------------------|----------------|
| 입력 | 질문 + 답변 + 정답/문맥 | 질문 + 답변만 |
| 정확도 | 높음 | 낮음 |
| 비용/데이터 | Ground truth 필요 | 없어도 됨 |
| 용도 | 회귀 평가 | Production 모니터링 |

## 작은 모델 vs 큰 모델

- 평가자는 보통 **production 모델보다 한 단계 더 강한 모델**을 쓴다 (편향이 적음).
- 자기보다 약한 모델 출력을 평가하면 일관성이 더 좋다.
- 단, 비용이 크면 작은 모델 + 다수결(여러 번 호출 후 평균)도 유효.

## 한계

- 사실 검증이 필요한 영역에서 평가자도 같은 환각을 공유할 수 있다.
- 미세한 차이(스타일, 톤)는 사람만큼 정밀하지 못함.
- **인간 평가와의 상관관계**를 미리 측정해 두고 신뢰 수준을 알고 써야 한다.

## 정량 지표와의 차이

- [[분류 평가 지표]]는 정답 라벨이 있을 때 좋다.
  - 예: 도구 선택이 맞았는지, 질병 분류가 맞았는지.
- [[회귀 평가 지표]]는 숫자 예측에 좋다.
  - 예: 주가, 수요량, 점수 예측이 얼마나 틀렸는지.
- LLM-as-Judge는 정답이 하나로 고정되지 않는 글쓰기형 결과에 좋다.
  - 예: 리포트가 충분한지, 근거를 잘 반영했는지, 답변이 친절한지.
- 그래서 실무에서는 세 가지를 섞는다.
  - 구조화된 부분은 코드로 채점하고, 자유 답변 부분은 judge LLM으로 본다.

## 관련

- [[AI 평가 지표]]
- [[Evaluation]] — 상위 개념.
- [[Reflection]] · [[Reflexion]] — 자기 평가에도 같은 메커니즘이 적용.
- [[Trajectory]] — 행동 시퀀스에 대한 LLM 채점.
