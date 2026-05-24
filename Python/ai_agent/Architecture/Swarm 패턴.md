- Swarm 패턴 = 중앙 매니저 없이, [[AI Agent|에이전트]]들이 서로에게 **handoff(인계)** 하면서 작업을 이어가는 [[Multi Agent|멀티 에이전트]] 구조.
- 2024년 OpenAI가 [Swarm](https://github.com/openai/swarm) 라이브러리로 발표하면서 용어가 굳어졌고, [[Strands Agents]]·[[LangGraph]]·[[AutoGen]] 등도 같은 패턴을 지원.

## 핵심 아이디어 — Handoff

- 각 에이전트는 평범한 [[Tool Calling|도구]]처럼 "다른 에이전트로 넘기기" 함수를 갖는다.
- LLM이 이 도구를 호출하면 **제어권이 그 에이전트로 넘어간다.**
- 대화 컨텍스트는 공유 — 마치 콜센터에서 담당자가 바뀌듯 자연스럽다.

## OpenAI Swarm 예제

```python
from swarm import Swarm, Agent
client = Swarm()

def transfer_to_refund():
    return refund_agent  # 다른 에이전트 반환 = handoff

triage = Agent(
    name="triage",
    instructions="문의 유형을 보고 적절한 에이전트로 넘겨라.",
    functions=[transfer_to_refund],
)
refund_agent = Agent(name="refund", instructions="환불을 처리한다.")

resp = client.run(agent=triage, messages=[{"role": "user", "content": "환불해줘"}])
print(resp.agent.name, resp.messages[-1]["content"])
```

## Strands 예제

```python
from strands import Agent, Swarm

researcher = Agent(name="researcher", ...)
writer = Agent(name="writer", ...)
swarm = Swarm(agents=[researcher, writer])
swarm("리서치하고 글 써줘")
```

## Supervisor와의 차이

| | [[Supervisor 패턴]] | Swarm |
|---|---|---|
| 라우팅 결정 | Supervisor가 중앙에서 | 각 에이전트가 자율 |
| 흐름 | 매니저 → 워커 → 매니저 | P2P, 그래프 |
| 디버깅 | 쉬움 | 어려움 |
| 자율성 | 낮음 | 높음 |

## 장점

- **분산 자율** — 도메인 전문가 에이전트들이 알아서 협업.
- **컨텍스트 자연 인계** — 콜센터·상담 시나리오에 잘 맞음.

## 단점

- 흐름이 비결정적이라 디버깅·평가가 어렵다.
- 종료 조건을 명확히 안 잡으면 핑퐁 무한루프.
- 어떤 에이전트가 어떤 능력을 가졌는지 LLM이 잘 알아야 — `description` 품질이 곧 동작 품질.

## 관련

- [[Multi Agent]] · [[Supervisor 패턴]] · [[Hierarchical Agent]].
- 구현: [[Strands Agents]], [[AutoGen]], [[LangGraph]].
