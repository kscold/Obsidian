- AutoGen은 Microsoft가 만든 [[Multi Agent|멀티 에이전트]] 프레임워크. **대화(conversation)** 를 1급 시민으로 두고, 에이전트들이 메시지를 주고받으며 협업한다.
- 핵심: 모든 게 `agent.send()` / `agent.receive()` 메시지 패턴으로 표현됨. 그래프나 워크플로우보다 **에이전트 간 채팅**에 가깝다.

## 두 가지 기본 에이전트

- `AssistantAgent` — LLM 기반 도우미.
- `UserProxyAgent` — 사람 역할 + 코드 실행. 자동/수동 응답 가능.

## 가장 단순한 예제 (AutoGen v0.4)

```python
from autogen_agentchat.agents import AssistantAgent
from autogen_agentchat.teams import RoundRobinGroupChat
from autogen_ext.models.openai import OpenAIChatCompletionClient

model_client = OpenAIChatCompletionClient(model="gpt-4o-mini")

planner = AssistantAgent("planner", model_client=model_client,
    system_message="작업을 세분화한다.")
coder = AssistantAgent("coder", model_client=model_client,
    system_message="파이썬 코드를 작성한다.")
critic = AssistantAgent("critic", model_client=model_client,
    system_message="코드를 검토하고 피드백한다.")

team = RoundRobinGroupChat([planner, coder, critic], max_turns=6)
await team.run(task="피보나치 함수를 만들어줘")
```

## Group Chat 패턴

- **RoundRobin** — 순서대로 발언.
- **Selector** — LLM이 다음 발언자를 동적으로 선택 ([[Supervisor 패턴]]에 가까움).
- **Swarm** — handoff로 다른 에이전트에 작업 위임 ([[Swarm 패턴]]).

## 코드 실행 — UserProxyAgent

```python
user_proxy = UserProxyAgent(
    "user_proxy",
    human_input_mode="NEVER",        # 자동 진행
    code_execution_config={"work_dir": "coding"},
)
# AssistantAgent가 코드 블록을 던지면, UserProxy가 실행해 결과 반환
```

- "LLM이 코드를 짜고, 다른 에이전트가 실행해 검증"하는 루프를 가장 자연스럽게 구현.

## v0.2 vs v0.4

- v0.4(2025년 안정화)는 비동기·타입 안전·이벤트 기반으로 재설계됐다.
- 기존 코드는 v0.2 API를 쓰는 경우가 많아 버전 확인 필수.

## 강점

- 코드 실행 + 멀티 에이전트 조합이 자연스럽다 — 자율 소프트웨어 엔지니어링 데모의 표준.
- Microsoft 리서치가 활발히 업데이트.

## 약점

- 학습 곡선이 있고 추상화가 자주 바뀐다.
- production 안정성은 [[LangGraph]] 쪽이 더 검증돼 있다.

## 관련

- [[CrewAI]] — 역할 기반 비슷한 결.
- [[Strands Agents]] — 더 라이트.
- [[Supervisor 패턴]] · [[Swarm 패턴]].
