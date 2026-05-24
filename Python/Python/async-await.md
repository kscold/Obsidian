- `async`/`await`는 파이썬의 **코루틴(coroutine)** 문법. I/O 대기 시간 동안 다른 작업을 할 수 있게 해서 [[LLM(Large Language Model)]] · DB · HTTP 같은 네트워크 호출을 효율적으로 처리한다.
- [[AI Agent|에이전트]]에서 여러 도구를 병렬 호출하거나 스트리밍 응답을 받을 때 거의 필수.

## 동기 vs 비동기

```python
import time, requests

# 동기 — 5초씩 직렬 → 총 15초
for url in urls:
    requests.get(url)

# 비동기 — 동시에 → 총 ~5초
import asyncio, httpx
async def fetch(url):
    async with httpx.AsyncClient() as c:
        return await c.get(url)

async def main():
    return await asyncio.gather(*[fetch(u) for u in urls])

asyncio.run(main())
```

## 핵심 키워드

- `async def` — 코루틴 함수 정의. 호출하면 코루틴 객체가 반환될 뿐 즉시 실행되지 않는다.
- `await` — 코루틴/awaitable을 기다린다. `async` 함수 안에서만 사용.
- `asyncio.run(coro)` — 이벤트 루프를 시작해 코루틴을 실행. 진입점에서 1번.
- `asyncio.gather(*coros)` — 여러 코루틴을 **병렬 실행**하고 결과 리스트를 반환.
- `asyncio.create_task(coro)` — 코루틴을 백그라운드 태스크로 스케줄.

## LLM 병렬 호출

```python
from openai import AsyncOpenAI
client = AsyncOpenAI()

async def ask(prompt: str) -> str:
    resp = await client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{"role": "user", "content": prompt}],
    )
    return resp.choices[0].message.content

async def main():
    answers = await asyncio.gather(
        ask("RAG가 뭐야?"),
        ask("LangGraph가 뭐야?"),
        ask("MCP가 뭐야?"),
    )
    for a in answers: print(a)
```

- 3번의 LLM 호출이 동시에 시작 → 가장 느린 응답 시간만큼만 걸린다.

## Streaming

```python
async for chunk in client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[...],
    stream=True,
):
    print(chunk.choices[0].delta.content or "", end="")
```

- 토큰이 도착하는 즉시 처리 — 사용자에게 첫 토큰까지의 지연(TTFT)이 크게 줄어든다.

## 흔한 함정

- **`async` 안에서 동기 블로킹 함수 호출** — `time.sleep`, `requests.get` 같은 건 이벤트 루프를 통째로 막는다. `asyncio.sleep`, `httpx.AsyncClient`로 대체.
- **CPU 바운드 작업** — async는 I/O 대기에만 효과. CPU 계산은 `ProcessPoolExecutor` 필요.
- **`asyncio.run`을 중첩** — 이벤트 루프 안에서 또 `run`하면 에러. 이미 안이라면 `await`만.

## 동기/비동기 양쪽 지원 — LangChain 컨벤션

- `chain.invoke()` (동기) ↔ `chain.ainvoke()` (비동기).
- `.stream()` ↔ `.astream()`, `.batch()` ↔ `.abatch()`.
- 함수 이름 앞에 `a` = async 버전.

## 관련

- [[제너레이터(Generator)]] — `yield` 기반 동기/비동기 스트림.
- [[Tool Calling]] — 병렬 도구 호출에 `gather` 활용.
