- CrewAI는 **역할(role)-기반 [[Multi Agent|멀티 에이전트]]** 프레임워크. 각 에이전트에게 직업(`role`)과 목표(`goal`), 백스토리(`backstory`)를 주고, 작업(`Task`)을 분배해 협업하게 한다.
- LangGraph보다 추상화가 한 단계 더 위 — "Crew를 짠다"는 비유가 그대로 코드에 드러난다.

## 예제

```python
from crewai import Agent, Task, Crew

researcher = Agent(
    role="시장 분석가",
    goal="최근 SaaS 트렌드를 정리",
    backstory="10년차 컨설턴트",
    tools=[search_tool],
)
writer = Agent(
    role="테크 작가",
    goal="분석 결과를 블로그 톤으로 정리",
    backstory="IT 매체 전문 기고가",
)

task_research = Task(
    description="2025년 상반기 SaaS 트렌드 top 5",
    expected_output="5개 항목과 근거",
    agent=researcher,
)
task_write = Task(
    description="블로그용 글 작성",
    expected_output="800자 한국어 글",
    agent=writer,
    context=[task_research],  # 앞 결과를 입력으로
)

crew = Crew(agents=[researcher, writer], tasks=[task_research, task_write])
result = crew.kickoff()
```

## Process 종류

- **Sequential** — 작업을 순서대로.
- **Hierarchical** — 매니저 LLM이 작업을 분배·검토 ([[Supervisor 패턴]]).

## 장점

- 역할 정의가 자연어로 명료해 도메인 전문가가 읽고 이해 가능.
- 협업 시나리오(분업, 검토, 리뷰)를 코드 거의 없이 짤 수 있다.

## 단점

- 내부 흐름이 불투명할 때가 있다 (LLM이 알아서 처리).
- 결정론적 [[Agent vs Workflow|워크플로우]]엔 부적합.
- 비용·지연 증가 — 에이전트마다 LLM 호출.

## 관련

- [[AutoGen]] — Microsoft의 비슷한 결.
- [[Strands Agents]] — 더 라이트한 멀티 에이전트.
- [[LangGraph]] — 더 정밀한 제어가 필요할 때.
