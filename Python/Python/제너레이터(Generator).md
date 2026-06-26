- 제너레이터(Generator) = `yield`를 쓰는 함수. 호출하면 값을 **하나씩 lazy하게** 만들어내는 **이터레이터**를 반환한다.
- [[LLM(Large Language Model)]] 응답을 토큰 단위로 흘려보내는 **스트리밍**, 대용량 데이터 청크 단위 처리, [[RAG(Retrieval-Augmented Generation)]] 인덱싱 파이프라인 등에서 핵심.

## 기본 형태

```python
def count_up(n):
    for i in range(n):
        yield i

g = count_up(3)
next(g)  # 0
next(g)  # 1
for v in g: print(v)   # 2
```

- 함수 안에 `yield`가 있으면 그 함수는 일반 함수가 아니라 **제너레이터 함수**가 된다.
- 호출해도 즉시 실행되지 않고, `next()`나 `for`로 소비할 때 한 단계씩 실행.

## 왜 쓰나

- **메모리 효율** — 전체 리스트를 메모리에 안 만들고 한 개씩 흘림.

```python
# 안 좋음: 1억 개 메모리 점유
nums = [i*i for i in range(10**8)]

# 좋음: 1개씩 생성
nums = (i*i for i in range(10**8))   # generator expression
```

## LLM 스트리밍

```python
from openai import OpenAI
client = OpenAI()

def stream_answer(prompt: str):
    resp = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{"role": "user", "content": prompt}],
        stream=True,
    )
    for chunk in resp:
        delta = chunk.choices[0].delta.content
        if delta:
            yield delta

for token in stream_answer("AI Agent란?"):
    print(token, end="", flush=True)
```

- 사용자는 첫 토큰을 거의 즉시 받기 시작 → 체감 응답 속도가 크게 좋아진다.

## 비동기 제너레이터

```python
async def astream(prompt):
    async for chunk in await client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{"role": "user", "content": prompt}],
        stream=True,
    ):
        delta = chunk.choices[0].delta.content
        if delta: yield delta

async for tok in astream("Hi"):
    print(tok, end="")
```

- [[async-await]]과 결합하면 FastAPI의 `StreamingResponse`로 그대로 전달 가능.

## yield from

```python
def chain(*iterables):
    for it in iterables:
        yield from it      # for v in it: yield v 와 동일
```

## send / throw / return

- 제너레이터는 단순 출력뿐 아니라 양방향 통신도 가능 (`gen.send(value)`).
- 대부분의 일반 애플리케이션에서는 잘 안 쓰고, 코루틴/상태머신 구현 시 등장.

## 청크 단위 RAG 인덱싱 (실전 예)

```python
def load_chunks(paths):
    for p in paths:
        with open(p) as f:
            text = f.read()
        for chunk in split(text, size=500, overlap=50):
            yield {"source": p, "text": chunk}

for batch in batched(load_chunks(paths), 100):
    vectordb.add_texts([c["text"] for c in batch],
                       metadatas=[{"source": c["source"]} for c in batch])
```

- 전체 코퍼스를 메모리에 안 올리고 100개씩 인덱싱.

## 관련

- [[async-await]] — 비동기 제너레이터.
- [[청킹(Chunking)]] — RAG 인덱싱 시 generator 자연스러움.
- [[LLM(Large Language Model)]] — 스트리밍 응답.
