- AI Agent는 [[LLM(Large Language Model)]]을 두뇌(reasoning engine)로 삼아 **도구(Tool)** 호출, **메모리(Memory)**, **계획(Planning)** 을 결합해 자율적으로 목표를 달성하는 시스템이다.
- 단발성 응답만 내놓는 일반 챗봇과 달리, 사용자의 요청을 분해해 어떤 행동을 할지 스스로 정하고, 도구로 외부 세계와 상호작용한 결과를 다시 추론에 반영하는 **[[Agentic Loop|루프]]** 구조를 가진다.
- 핵심 구성요소: [[LLM(Large Language Model)]], [[Tool Calling]], [[Memory]], [[Planning]], [[Reflection]].
- 대표 패턴: [[ReAct 패턴]], [[Plan-and-Execute]], [[Reflexion]], [[Self-Improving Agent]].
- 대표 아키텍처: [[Single Agent]], [[Multi Agent]], [[Supervisor 패턴]], [[Swarm 패턴]], [[Hierarchical Agent]], [[Agent Graph]], [[Agent as Tool]].
- 대표 프레임워크: [[LangChain]], [[LangGraph]], [[LlamaIndex]], [[Strands Agents]], [[CrewAI]], [[AutoGen]].

## 목차

### Concepts (개념 · 패턴)
- [[AI Agent란]]
- [[LLM과 Agent의 차이]]
- [[Agent vs Workflow]]
- [[Agentic Loop]]
- [[ReAct 패턴]]
- [[Plan-and-Execute]]
- [[Reflexion]]
- [[Self-Improving Agent]]
- [[Routing Workflow]]
- [[Evaluator-Optimizer]]
- [[A2A 프로토콜]]
- [[BERT]]

### RAG (검색 증강 생성)
- [[RAG(Retrieval-Augmented Generation)]]
- [[임베딩(Embedding)]]
- [[코사인 유사도]]
- [[벡터 데이터베이스]]
- [[청킹(Chunking)]]
- [[Hybrid Search]]
- [[Reranking]]
- [[GraphRAG]]
- [[Knowledge Graph]]
- [[Ontology]]
- [[AI-Ready Data]]
- [[Agentic RAG]]

### Frameworks (프레임워크)
- [[LangChain]]
- [[LangGraph]]
- [[LangGraph StateGraph]]
- [[LlamaIndex]]
- [[Strands Agents]]
- [[CrewAI]]
- [[AutoGen]]

### Architecture (아키텍처 · 토폴로지)
- [[LangGraph 워크플로우 아키텍처]]
- [[Single Agent]]
- [[Multi Agent]]
- [[Supervisor 패턴]]
- [[Swarm 패턴]]
- [[Hierarchical Agent]]
- [[Agent Graph]]
- [[Agent as Tool]]
- [[Sub-LLM as Tool]]

### Components (핵심 구성요소)
- [[Tool Calling]]
- [[LangChain @tool]]
- [[LLM Tool Selection]]
- [[LangGraph ToolNode]]
- [[Memory]]
- [[Planning]]
- [[Reflection]]
- [[System Prompt]]
- [[Prompt Engineering]]
- [[Structured Output]]
- [[Intent Classification]]
- [[Streaming]]
- [[Trajectory]]
- [[Human-in-the-loop]]
- [[Evaluation]]
- [[LangSmith]]
- [[AI 평가 지표]]
- [[텍스트 생성 평가 지표]]
- [[LLM-as-Judge]]
- [[Guardrails]]
- [[Observability]]
- [[Cost와 Token]]
- [[LLM Provider 추상화]]
- [[MCP(Model Context Protocol)]]
- [[SKILL]]

### LangGraph 실습
- [[AI Agent 실습 MOC]]
- [[LangGraph 개념 MOC]]
- [[LangGraph 문법 치트시트]]
- [[LangGraph State]]
- [[LangGraph Node]]
- [[LangGraph Edge]]
- [[Workflow Node vs Tool]]
- [[Retrieve-Generate 패턴]]

### Python 문법 (인접 폴더)
- [[LLM(Large Language Model)]]
- [[async-await]]
- [[데코레이터(Decorator)]]
- [[typing 모듈]]
- [[Pydantic]]
- [[제너레이터(Generator)]]
- [[dataclass]]
- [[context manager]]
- [[gRPC]]
