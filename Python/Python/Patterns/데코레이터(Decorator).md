- 데코레이터(Decorator) = **함수를 인자로 받아 함수를 반환하는 함수**. `@decorator` 문법으로 다른 함수에 기능을 덧붙일 때 쓴다.
- [[AI Agent|에이전트]] 코드에서 [[Tool Calling|도구 정의]](`@tool`), 캐싱(`@lru_cache`), 라우팅(`@app.get`), 검증(`@validator`) 등에 두루 쓰인다.

## 기본 형태

```python
def log(fn):
    def wrapper(*args, **kwargs):
        print(f"calling {fn.__name__}")
        result = fn(*args, **kwargs)
        print(f"done {fn.__name__}")
        return result
    return wrapper

@log
def add(a, b): return a + b

add(1, 2)
# calling add
# done add
# → 3
```

- `@log` = `add = log(add)`의 syntactic sugar.

## 인자 받는 데코레이터

```python
def retry(times: int):
    def deco(fn):
        def wrapper(*args, **kwargs):
            for i in range(times):
                try: return fn(*args, **kwargs)
                except Exception:
                    if i == times - 1: raise
        return wrapper
    return deco

@retry(times=3)
def fetch(url): ...
```

## `functools.wraps`로 메타데이터 보존

```python
from functools import wraps

def log(fn):
    @wraps(fn)   # 이걸 안 쓰면 wrapper.__name__ == "wrapper"가 된다
    def wrapper(*args, **kwargs):
        return fn(*args, **kwargs)
    return wrapper
```

## 자주 쓰는 표준 데코레이터

- `@staticmethod` / `@classmethod` — 인스턴스 없이 호출 가능한 메서드.
- `@property` — 메서드를 속성처럼 호출.
- `@functools.lru_cache(maxsize=128)` — 결과 캐싱.
- `@functools.cached_property` — 인스턴스 속성 캐싱.
- `@dataclass` — 클래스에 `__init__`, `__repr__` 자동 생성.

## Agent 코드에서의 활용

```python
# LangChain — 도구 등록
from langchain_core.tools import tool

@tool
def get_weather(city: str) -> str:
    """도시 날씨를 반환한다."""
    return weather_api.fetch(city)

# Strands — 도구 등록
from strands.tools import tool

@tool
def search_web(query: str) -> str:
    """웹 검색을 수행한다."""
    return tavily.search(query)

# FastAPI — Agent 엔드포인트
@app.post("/chat")
async def chat(req: ChatRequest):
    return await agent.ainvoke(req.message)
```

## 클래스 데코레이터

```python
def singleton(cls):
    instances = {}
    def get():
        if cls not in instances:
            instances[cls] = cls()
        return instances[cls]
    return get

@singleton
class Config: ...
```

## 관련

- [[typing 모듈]] — 데코레이터 시그니처 타입 힌트.
- [[Pydantic]] — `@validator`, `@field_validator`.
- [[Tool Calling]] — `@tool` 데코레이터로 도구 등록.
