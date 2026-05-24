- LlamaIndex는 **[[RAG(Retrieval-Augmented Generation)]]에 특화된** 파이썬 프레임워크. "내 데이터를 LLM에 잘 먹이는 것"이 1순위 설계 목표다.
- [[LangChain]]이 범용 빌딩 블록이라면, LlamaIndex는 데이터 인덱싱·검색·평가 쪽 추상화가 훨씬 풍부하다.

## 핵심 개념

| 용어 | 의미 |
|------|------|
| `Document` | 원본 입력 단위 |
| `Node` | 청크 + 메타데이터 + 관계 |
| `Index` | Node 컬렉션에 대한 검색 구조 |
| `Retriever` | Index에서 관련 Node 선택 |
| `QueryEngine` | Retriever + LLM = 한 번에 답 |
| `ChatEngine` | 대화형 RAG |
| `Agent` | Tool 호출 가능한 LlamaIndex 에이전트 |

## 가장 단순한 예제

```python
from llama_index.core import VectorStoreIndex, SimpleDirectoryReader

docs = SimpleDirectoryReader("./data").load_data()
index = VectorStoreIndex.from_documents(docs)

query_engine = index.as_query_engine()
print(query_engine.query("환불 정책이 뭐야?"))
```

- 4줄로 인덱싱+검색+생성이 끝난다 — 가장 큰 매력.

## Index 종류

- **VectorStoreIndex** — 가장 흔함. [[임베딩(Embedding)]] 기반.
- **SummaryIndex** — 모든 노드를 LLM에 넘겨 요약 — 작은 코퍼스용.
- **TreeIndex** — 계층적 요약 → 빠른 탐색.
- **KeywordTableIndex** — 키워드 기반.
- **PropertyGraphIndex** — [[GraphRAG]] 스타일 그래프 인덱스.

## 고급 패턴

### Routing — 질의에 따라 다른 인덱스로

```python
from llama_index.core.query_engine import RouterQueryEngine

router = RouterQueryEngine.from_defaults(
    query_engine_tools=[summary_tool, vector_tool]
)
```

### Sub-question Decomposition — 복합 질문 분해

```python
from llama_index.core.query_engine import SubQuestionQueryEngine
sqe = SubQuestionQueryEngine.from_defaults(query_engine_tools=[...])
sqe.query("A 회사와 B 회사의 매출을 비교해줘")
# → "A 매출은?", "B 매출은?" 으로 분해 후 통합
```

### Agent (도구 호출)

```python
from llama_index.core.agent.workflow import FunctionAgent

agent = FunctionAgent(
    tools=[multiply, add],
    llm=OpenAI(model="gpt-4o-mini"),
    system_prompt="너는 계산 도우미다.",
)
await agent.run("3과 4를 곱하고 5를 더해줘")
```

## LlamaParse — PDF·복잡한 문서

- LlamaIndex 팀이 만든 별도 서비스로, 표·차트·복잡한 레이아웃을 잘 추출. RAG 품질에 직접적 영향.

## 평가 도구

- `llama-index-evaluations` 모듈에 Faithfulness, Relevancy, Correctness 평가가 내장.

## LangChain vs LlamaIndex 선택

| 상황 | 추천 |
|------|------|
| 복잡한 RAG, 인덱스 다양성 필요 | LlamaIndex |
| 멀티 에이전트·복잡한 워크플로우 | [[LangGraph]] |
| 광범위한 외부 통합 (DB, API, SaaS) | [[LangChain]] |
| 빠른 PoC | LlamaIndex (4줄로 끝남) |

- 둘을 섞어 쓰는 것도 흔하다 — 인덱싱은 LlamaIndex, 에이전트는 LangGraph.
