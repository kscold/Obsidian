- [[LLM(Large Language Model)]]은 입력 텍스트에 대해 다음 토큰을 예측하는 **함수**이고, [[AI Agent]]는 그 함수를 두뇌처럼 쓰면서 **도구·메모리·루프**를 더한 **시스템**이다.
- LLM은 입력 → 출력으로 끝나지만, Agent는 출력을 다시 환경(도구)에 반영한 뒤 그 결과를 입력으로 되돌리는 **피드백 루프**를 갖는다.

## 비교표

| 구분 | LLM | Agent |
|------|-----|-------|
| 본질 | 텍스트 생성 모델 | LLM + 도구 + 메모리 + 루프 |
| 외부 정보 | 학습 시점 데이터만 | [[Tool Calling]]로 실시간 조회 가능 |
| 상태 | Stateless | [[Memory]]로 stateful |
| 실행 | 단방향 1회 | 다단계 루프, 자율 종료 |
| 부작용 | 텍스트 출력만 | 메일 발송·DB 변경 등 실세계 영향 가능 |
| 예측 가능성 | 비교적 일관 | 같은 입력에도 경로 분기 가능 |

## 코드 비교

```python
# (1) 순수 LLM — Stateless
from openai import OpenAI
client = OpenAI()
res = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "내 주문 상태 알려줘"}]
)
# → "주문 번호를 알려주세요" 정도가 한계

# (2) Agent — 도구로 실제 DB 조회까지
from langchain.agents import create_agent
tools = [order_lookup_tool, refund_tool]
agent = create_agent(llm, tools)
res = agent.invoke({"user_id": 42, "input": "내 주문 상태 알려줘"})
# → user_id로 order_lookup_tool 호출 → DB에서 실제 상태 가져와 응답
```

## "Agent 같은 LLM"과 "진짜 Agent"

- 최근 GPT-4o, Claude처럼 도구 호출이 내장된 LLM은 한 번의 요청으로도 도구를 부르긴 한다. 그러나 **여러 단계의 추론·재시도·계획 분해** 가 들어가야 비로소 Agent라고 부른다.
- 단순히 `tools=[...]` 만 붙인 1회 호출은 [[Tool Calling]]일 뿐, 아직 Agent는 아니다.

## 정리

- LLM = 부품, Agent = 그 부품을 쓰는 완성품.
- Agent를 만들 때 LLM을 "선택"하는 것이지, LLM을 "확장"하는 게 아니다 — LLM은 그대로고, 주변 인프라가 Agent를 만든다.
