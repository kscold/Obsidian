- Streaming = [[LLM(Large Language Model)]]이 생성하는 토큰을 **다 끝날 때까지 기다리지 않고 도착하는 즉시 사용자에게 흘려보내는** 응답 방식.
- 체감 응답 속도(TTFT, Time-To-First-Token)가 크게 줄어 UX가 결정적으로 좋아진다. [[AI Agent|에이전트]] UI의 사실상 표준.

## 일반 호출 vs 스트리밍

```python
# 일반 — 완성까지 기다림 (예: 8초)
resp = client.chat.completions.create(model="gpt-4o-mini", messages=msgs)
print(resp.choices[0].message.content)

# 스트리밍 — 첫 토큰까지 0.5초, 이후 점진적
stream = client.chat.completions.create(model="gpt-4o-mini", messages=msgs, stream=True)
for chunk in stream:
    delta = chunk.choices[0].delta.content
    if delta: print(delta, end="", flush=True)
```

## 무엇이 스트리밍되는가

- **텍스트 토큰** — 가장 흔함.
- **Tool call 인자** — [[Tool Calling]] 호출 시 인자 JSON도 토큰 단위로.
- **State 변화** ([[LangGraph]]) — 그래프 노드 단계마다 stream.
- **Trajectory** ([[ReAct 패턴]]) — Thought/Action/Observation 단위.

## LangChain — astream

```python
async for chunk in chain.astream({"input": "..."}):
    yield chunk        # FastAPI StreamingResponse로 그대로
```

## LangGraph — 4가지 모드

```python
async for event in graph.astream({"messages": [...]}, stream_mode="values"):
    ...
# stream_mode 옵션
# - "values"   : 매 노드 후 전체 state
# - "updates"  : 변경된 부분만
# - "messages" : LLM 토큰 단위
# - "debug"    : 모든 이벤트
```

## FastAPI로 사용자에게 전달

```python
from fastapi.responses import StreamingResponse

@app.post("/chat")
async def chat(req: ChatRequest):
    async def gen():
        async for tok in stream_answer(req.message):
            yield f"data: {tok}\n\n"   # Server-Sent Events
    return StreamingResponse(gen(), media_type="text/event-stream")
```

- 프론트는 `EventSource` 또는 fetch + `getReader()`로 수신.

## 흔한 함정

- **중간에 에러** — 이미 일부 토큰을 보낸 뒤 실패하면 정정 메시지를 어떻게 보낼지 미리 정해둘 것.
- **버퍼링** — nginx·CDN이 응답을 모아 한꺼번에 보내면 스트리밍 의미 없음. `X-Accel-Buffering: no` 헤더.
- **토큰 카운트** — 스트리밍 중 token usage가 없거나 마지막 chunk에만 있음. 공급자별 차이 확인.
- **취소** — 사용자가 페이지를 떠나면 generator를 멈춰야 토큰 비용이 안 새 나간다.

## 관련

- [[제너레이터(Generator)]] · [[async-await]] — 스트리밍의 파이썬 기초.
- [[LLM(Large Language Model)]] — 모델별 스트리밍 형식.
- [[Observability]] — 스트림 이벤트 트레이싱.
