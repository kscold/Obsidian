- `dataclass` = 파이썬 표준 라이브러리(`dataclasses`)의 데코레이터. **`__init__`, `__repr__`, `__eq__` 같은 보일러플레이트를 자동 생성**해주는 가벼운 데이터 컨테이너.
- [[Pydantic]]보다 가볍지만 런타임 검증이 없다. 내부 자료구조에 자주 쓰고, 외부 입출력엔 Pydantic이 안전.

## 기본 사용

```python
from dataclasses import dataclass, field

@dataclass
class Message:
    role: str
    content: str
    tokens: int = 0
    metadata: dict = field(default_factory=dict)

m = Message(role="user", content="안녕")
# 자동 생성:
# __init__(role, content, tokens=0, metadata=...)
# __repr__: Message(role='user', content='안녕', tokens=0, metadata={})
# __eq__: 모든 필드 동등 비교
```

## 옵션

```python
@dataclass(frozen=True)        # 불변 (= NamedTuple과 비슷)
class Coord:
    x: int
    y: int

@dataclass(slots=True)         # __slots__ 생성 → 메모리 절약, 속도 ↑
class Tight:
    a: int
    b: int

@dataclass(order=True)         # __lt__ 등 비교 메서드 자동
class Score:
    value: int
```

## `field()` 옵션

```python
@dataclass
class Config:
    name: str
    tags: list[str] = field(default_factory=list)   # mutable default 안전 패턴
    secret: str = field(repr=False)                 # repr에서 숨김
    cache: dict = field(default_factory=dict,
                       compare=False)               # 비교에서 제외
```

- mutable default(`tags: list = []`)는 함정 — 모든 인스턴스가 같은 리스트 공유. 항상 `field(default_factory=list)`.

## post-init 훅

```python
@dataclass
class User:
    name: str
    upper_name: str = ""

    def __post_init__(self):
        self.upper_name = self.name.upper()
```

## dataclass vs Pydantic vs TypedDict

| | dataclass | [[Pydantic]] | TypedDict |
|--|-----------|--------------|-----------|
| 런타임 검증 | X | O | X |
| 직렬화/JSON | 수동 | 내장 (`model_dump`) | 수동 |
| 무게 | 가벼움 | 무거움 | 가장 가벼움 |
| 자동 변환(coercion) | X | O | X |
| 용도 | 내부 자료구조 | 외부 입출력·LLM 출력 | dict 모양 타입 힌트만 |

## 에이전트 코드에서의 활용

```python
@dataclass(slots=True, frozen=True)
class ToolCall:
    name: str
    args: dict

@dataclass
class AgentTurn:
    user_message: str
    tool_calls: list[ToolCall] = field(default_factory=list)
    final_answer: str | None = None
    tokens_in: int = 0
    tokens_out: int = 0
```

- 트레이스·히스토리·이벤트 같은 **내부에서만 흐르는 자료구조**에 적합.
- LLM 호출·API I/O에는 [[Pydantic]]을 쓰자 — 검증과 직렬화를 무료로 받음.

## 관련

- [[Pydantic]] · [[typing 모듈]] · [[데코레이터(Decorator)]].
