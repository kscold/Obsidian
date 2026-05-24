- `typing` = 파이썬의 표준 타입 힌트 모듈. 런타임 동작은 바꾸지 않지만, mypy·Pyright 같은 정적 검사기와 IDE 자동완성, [[Pydantic]] 같은 라이브러리가 이를 활용한다.
- [[AI Agent|에이전트]] 코드는 [[Tool Calling|도구 시그니처]]가 곧 LLM이 읽는 매뉴얼이므로, 타입 힌트가 곧 문서이자 검증 수단이 된다.

## 기본 타입

```python
x: int = 1
y: float = 1.0
name: str = "kim"
flags: bool = True
items: list[str] = ["a", "b"]
mapping: dict[str, int] = {"a": 1}
pair: tuple[int, str] = (1, "x")
maybe: int | None = None        # Python 3.10+
```

## 자주 쓰는 special form

```python
from typing import Optional, Union, Any, Literal, Callable, TypedDict, Annotated

Optional[int]              # = int | None
Union[int, str]            # = int | str
Literal["a", "b"]          # 정확히 "a" 또는 "b"
Callable[[int, int], int]  # (int, int) -> int 함수
Any                        # 검사 비활성화 (탈출구)
```

## TypedDict — 딕셔너리 키별 타입

```python
from typing import TypedDict, Annotated

class AgentState(TypedDict):
    messages: list
    user_id: str
    counter: int
```

- [[LangGraph]]의 `State` 정의가 보통 TypedDict.

## Annotated — 메타데이터 동봉

```python
from typing import Annotated
from langgraph.graph.message import add_messages

class State(TypedDict):
    messages: Annotated[list, add_messages]   # 두 번째 인자는 reducer
```

- 타입 자체에는 영향 없지만, 프레임워크가 메타데이터를 읽어 동작을 바꾼다 (LangGraph reducer, FastAPI Depends 등).

## Generic — 타입 변수

```python
from typing import TypeVar, Generic

T = TypeVar("T")

class Stack(Generic[T]):
    def __init__(self): self.items: list[T] = []
    def push(self, x: T) -> None: self.items.append(x)
    def pop(self) -> T: return self.items.pop()

s: Stack[int] = Stack()
```

## Protocol — 덕 타이핑 + 타입 검사

```python
from typing import Protocol

class Embeddable(Protocol):
    def embed(self, text: str) -> list[float]: ...

def index(model: Embeddable, texts: list[str]) -> None: ...
# 명시적 상속 없이도 embed 메서드만 있으면 통과
```

- [[LangChain]]의 Runnable, [[LlamaIndex]]의 Embedding 등이 Protocol에 가깝다.

## Tool 시그니처 → JSON Schema

```python
from langchain_core.tools import tool

@tool
def get_weather(city: str, units: Literal["c", "f"] = "c") -> str:
    """도시의 날씨를 반환한다."""
    ...
```

- `city: str`, `units: Literal["c", "f"]`가 자동으로 JSON Schema의 `enum`까지 변환된다 → LLM이 잘못된 값을 못 넣음.

## 정적 검사 도구

- **mypy** — 가장 보수적·표준적.
- **Pyright** (VSCode Pylance 기반) — 빠르고 추론이 강력.
- **Ruff** — lint + 일부 타입 체크.

## 관련

- [[Pydantic]] — typing 힌트를 런타임 검증으로 확장.
- [[Tool Calling]] — 타입 힌트가 LLM에 노출되는 스키마.
- [[데코레이터(Decorator)]] — 시그니처 보존 패턴.
