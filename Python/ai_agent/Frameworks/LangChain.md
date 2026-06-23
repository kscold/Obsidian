- LangChain은 [[LLM(Large Language Model)]] 애플리케이션을 만들 때 자주 쓰는 빌딩 블록(프롬프트, 모델, 도구, 메모리, 리트리버, 출력 파서 등)을 **표준 인터페이스**로 묶은 가장 대중적인 파이썬/JS 프레임워크다.
- 2022년 출시. 초기에는 [[AI Agent|에이전트]] 자체를 만들었지만, 이제는 에이전트 그래프는 [[LangGraph]]가 맡고, LangChain은 **체인·통합 계층** 역할이 더 강하다.

## 핵심 추상화

| 컴포넌트 | 역할 |
|----------|------|
| `ChatModel` | LLM 호출 래퍼 (OpenAI, Anthropic, Bedrock, Ollama 등) |
| `PromptTemplate` | 변수 치환 가능한 프롬프트 |
| `OutputParser` | LLM 출력 → 구조화된 객체 |
| `Tool` | 함수를 LLM이 호출 가능한 도구로 변환 |
| `Retriever` | [[RAG(Retrieval-Augmented Generation)]] 검색 인터페이스 |
| `Memory` | 대화 이력 보관 |
| `Runnable` / LCEL | 컴포넌트를 `|`로 연결하는 표현식 |

## LCEL (LangChain Expression Language)

```python
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

prompt = ChatPromptTemplate.from_template("{topic}에 대해 한 문단으로 설명해줘")
model = ChatOpenAI(model="gpt-4o-mini")

chain = prompt | model | StrOutputParser()
chain.invoke({"topic": "RAG"})
```

- `|`(pipe)는 함수 합성. 입력이 좌측에서 우측으로 흐른다.
- 모든 `Runnable`은 `.invoke`, `.stream`, `.batch`, `.ainvoke`(비동기)를 공통 인터페이스로 제공.

## Tool 정의

```python
from langchain_core.tools import tool

@tool
def get_weather(city: str) -> str:
    """주어진 도시의 현재 날씨를 반환한다."""
    return weather_api.fetch(city)
```

- docstring과 타입힌트가 LLM에게 보여지는 도구 설명이 됨 → 잘 쓸수록 호출 정확도 ↑.

## RAG 한 줄 예제

```python
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings, ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate

vectordb = Chroma(persist_directory="./db", embedding_function=OpenAIEmbeddings())
retriever = vectordb.as_retriever()

prompt = ChatPromptTemplate.from_template("""
다음 컨텍스트만 사용해서 답해라.
컨텍스트: {context}
질문: {question}
""")

chain = (
    {"context": retriever, "question": lambda x: x}
    | prompt
    | ChatOpenAI()
    | StrOutputParser()
)
chain.invoke("환불 정책?")
```

## LangChain 생태계

- **langchain-core** — 추상화·인터페이스만 (가벼움).
- **langchain** — 체인·리트리버·에이전트 기본 구현.
- **langchain-community** — 외부 통합(벡터DB, 로더 등).
- **langchain-openai / anthropic / aws / google** — 공급사별 패키지.
- **[[LangGraph]]** — 상태 머신 기반 에이전트. 복잡한 분기, 반복, [[Human-in-the-loop]], [[LangGraph Checkpointer|checkpointing]]은 이쪽이 담당한다.
- **[[LangSmith]]** — 트레이싱·평가·프롬프트 관리 플랫폼.

## 비판과 대안

- "추상화가 과하다", "버전 호환성이 자주 깨진다"는 비판이 있음.
- 단순 케이스는 OpenAI SDK 직접 호출이나 [[LlamaIndex]], [[Strands Agents]]가 더 가벼움.
- 그래도 통합 폭이 압도적이라 PoC·프로토타이핑엔 여전히 1순위.
