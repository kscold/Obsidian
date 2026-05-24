- Pydantic = 파이썬에서 가장 널리 쓰이는 **데이터 검증·직렬화 라이브러리**. [[typing 모듈|타입 힌트]]를 런타임 검증으로 확장한다.
- [[AI Agent|에이전트]] 생태계에서 LLM의 **structured output**, [[Tool Calling|도구 파라미터]] 정의, [[LangGraph]] State, FastAPI 요청 모델 등 거의 모든 곳에 등장.
- v2(2023~) 기준으로 작성. v1과는 API가 다름.

## 기본 모델

```python
from pydantic import BaseModel, Field

class User(BaseModel):
    id: int
    name: str
    email: str | None = None
    age: int = Field(ge=0, le=150)

u = User(id=1, name="kim", age=20)
# u.id == 1, u.model_dump() == {"id":1, "name":"kim", "email":None, "age":20}

User(id="1", name="kim", age=20)   # 문자열 "1" → int 1 자동 변환 (coercion)
User(id=1, name="kim", age=-1)     # ValidationError — age >= 0 위반
```

## Validator

```python
from pydantic import BaseModel, field_validator, model_validator

class Trip(BaseModel):
    start: int
    end: int

    @field_validator("end")
    @classmethod
    def end_after_start(cls, v, info):
        if v <= info.data["start"]:
            raise ValueError("end must be > start")
        return v

    @model_validator(mode="after")
    def check_consistency(self):
        # 객체 전체 검증
        return self
```

## LLM structured output (가장 강력한 활용)

```python
from openai import OpenAI
from pydantic import BaseModel

class Recipe(BaseModel):
    name: str
    ingredients: list[str]
    steps: list[str]
    cook_time_min: int

client = OpenAI()
resp = client.beta.chat.completions.parse(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "김치찌개 레시피 알려줘"}],
    response_format=Recipe,
)
recipe: Recipe = resp.choices[0].message.parsed
# 이미 검증된 구조화 객체 — KeyError나 ValueError 걱정 없음
```

## LangChain의 `with_structured_output`

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-4o-mini")
structured = llm.with_structured_output(Recipe)
recipe = structured.invoke("김치찌개 레시피 알려줘")
```

## Tool 파라미터 정의

```python
from langchain_core.tools import tool
from pydantic import BaseModel, Field

class SearchInput(BaseModel):
    query: str = Field(description="검색어")
    top_k: int = Field(default=5, ge=1, le=20)

@tool("search", args_schema=SearchInput)
def search(query: str, top_k: int = 5) -> list[str]:
    """검색을 수행한다."""
    ...
```

- Pydantic Field의 `description`이 LLM에 그대로 전달된다 → 호출 정확도 향상.

## 직렬화

```python
u.model_dump()        # → dict
u.model_dump_json()   # → JSON 문자열
User.model_validate(d)        # dict → 모델
User.model_validate_json(s)   # JSON 문자열 → 모델
```

## Config

```python
from pydantic import BaseModel, ConfigDict

class Strict(BaseModel):
    model_config = ConfigDict(extra="forbid", frozen=True)
    # 정의되지 않은 키 거부, 인스턴스 불변
```

## 자주 쓰는 패턴

- **API 입출력 스키마** — FastAPI의 모든 요청/응답.
- **설정 클래스** — `BaseSettings` (별도 `pydantic-settings` 패키지)로 환경변수 로딩.
- **상태 머신 State** — [[LangGraph]] 그래프 State.
- **LLM 출력 스키마** — 위 structured output.

## 관련

- [[typing 모듈]] — 기반 타입 시스템.
- [[데코레이터(Decorator)]] — `@field_validator` 등.
- [[Tool Calling]] — args_schema로 정밀 제어.
