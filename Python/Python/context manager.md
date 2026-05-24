- Context Manager = `with` 문에 쓰는 객체. **진입(`__enter__`)에서 자원을 얻고, 종료(`__exit__`)에서 정확히 해제**되는 RAII 패턴.
- [[AI Agent|에이전트]] 코드에서 자주 등장 — 트레이싱 span, DB 트랜잭션, 파일/소켓, 임시 환경변수, [[async-await|비동기]] 클라이언트.

## 기본 사용

```python
with open("file.txt") as f:    # __enter__: open, __exit__: close
    data = f.read()
# 여기서 f는 자동 close
```

## 직접 구현 — 클래스 방식

```python
class Timer:
    def __enter__(self):
        self.start = time.time()
        return self
    def __exit__(self, exc_type, exc, tb):
        self.elapsed = time.time() - self.start
        return False    # 예외 재발생

with Timer() as t:
    do_work()
print(t.elapsed)
```

## `contextlib.contextmanager` — 함수형

```python
from contextlib import contextmanager

@contextmanager
def timer(label):
    start = time.time()
    try:
        yield
    finally:
        print(f"{label}: {time.time()-start:.2f}s")

with timer("llm_call"):
    llm.invoke(prompt)
```

- 가독성 좋고 자주 쓰는 패턴.

## 비동기 — async with

```python
import httpx

async def fetch():
    async with httpx.AsyncClient() as client:
        return await client.get("...")
```

- [[async-await|asyncio]] 친화. `__aenter__` / `__aexit__`를 구현하거나 `@asynccontextmanager` 사용.

## 트레이싱·관측 — Span as Context Manager

```python
from opentelemetry import trace
tracer = trace.get_tracer(__name__)

with tracer.start_as_current_span("agent.invoke") as span:
    span.set_attribute("user_id", uid)
    result = agent.invoke(...)
```

- [[Observability]]에서 거의 표준 형태.

## DB 트랜잭션

```python
with db.session() as s:
    s.execute("INSERT ...")
    # 예외 없으면 commit, 있으면 rollback
```

## 다중 자원 — ExitStack

```python
from contextlib import ExitStack

with ExitStack() as stack:
    files = [stack.enter_context(open(p)) for p in paths]
    # 모든 파일이 끝에 함께 닫힘
```

- 동적으로 컨텍스트 수가 정해질 때.

## 흔한 함정

- `__exit__`에서 True를 반환하면 예외가 **삼켜진다** — 의도한 게 아니면 항상 False.
- `with` 안에서 `return`해도 `__exit__`는 실행됨 (정상).
- 비동기 컨텍스트를 동기 `with`로 쓰면 동작 안 함.

## 관련

- [[데코레이터(Decorator)]] · [[async-await]] · [[Observability]].
