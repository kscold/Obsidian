- Strands Agents는 **AWS가 2025년 오픈소스로 공개한 경량 [[AI Agent|에이전트]] 프레임워크**. "model-driven, 코드 적게(low-code)"가 슬로건.
- 핵심 철학: **LLM의 도구 호출 능력을 그대로 믿고**, 프레임워크는 가능한 한 비켜선다. 그래서 [[LangGraph]] 처럼 그래프를 짜지 않고, 도구 리스트만 주면 LLM이 알아서 부른다.

## 가장 단순한 예제

```python
from strands import Agent

agent = Agent(
    model="anthropic.claude-3-5-sonnet-20241022-v2:0",  # Bedrock model
    tools=[search_web, read_file, write_file],
    system_prompt="너는 리서치 어시스턴트다.",
)

result = agent("최근 한 달간 RAG 논문 3개를 요약해줘")
```

- 별도 그래프, 별도 노드 정의 없음. 도구 함수만 넘기면 끝.

## Tool 정의

```python
from strands.tools import tool

@tool
def search_web(query: str) -> str:
    """웹에서 검색한다."""
    return tavily.search(query)
```

- 함수 시그니처 + docstring이 LLM 프롬프트에 들어감.

## 멀티 에이전트 ([[Swarm 패턴]])

```python
from strands import Agent, Swarm

researcher = Agent(name="researcher", tools=[search_web], model=...)
writer = Agent(name="writer", tools=[write_file], model=...)

swarm = Swarm(agents=[researcher, writer])
swarm("리서치하고 보고서 써줘")
# → 에이전트들이 서로에게 작업을 넘기며 협업
```

- Strands는 [[Swarm 패턴]] · [[Supervisor 패턴]] · [[Hierarchical Agent|Hierarchical]] 토폴로지를 빌트인 지원.

## 특징

- **AWS Bedrock 친화** — 모델 ID만 적으면 Claude·Llama·Mistral·Nova 다 호출.
- OpenAI/Anthropic API 직접 호출도 지원.
- 옵션이 적어 **러닝 커브가 LangGraph보다 훨씬 완만**.
- 그러나 그만큼 정밀 제어는 어렵다 — 복잡한 분기·휴먼 인 더 루프가 필요하면 [[LangGraph]]가 낫다.

## AgentCore와의 관계

- **AWS AgentCore** = Strands로 만든 에이전트를 운영(런타임, 메모리, 식별, 게이트웨이, 평가)하는 매니지드 플랫폼.
- Strands는 SDK, AgentCore는 그 SDK로 만든 앱을 실행하는 인프라 — 둘은 별개지만 잘 맞물린다.

## 언제 쓰나

- **AWS Bedrock 환경**에서 빠르게 에이전트를 띄우고 싶을 때.
- 도구 호출이 핵심이고, 복잡한 그래프가 필요 없을 때.
- 멀티 에이전트를 코드 적게 시도해 보고 싶을 때.

## 관련

- [[LangGraph]] — 더 정밀한 제어가 필요하면.
- [[CrewAI]] · [[AutoGen]] — 멀티 에이전트 대안.
