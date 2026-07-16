---
title: Async Resource Lifecycle
created: 2026-07-15
tags:
  - ai-agent
  - python
  - async
  - infrastructure
---

# Async Resource Lifecycle

Async Resource Lifecycle은 DB driver, HTTP client, LLM client처럼 오래 살아 있는 비동기 자원을 **어느 이벤트 루프에서 만들고, 어떻게 공유하고, 언제 닫을지** 정하는 설계다. `async` 함수를 쓴다고 자원 수명 문제가 자동으로 해결되지는 않는다.

## 기본 원칙

```mermaid
flowchart LR
    Startup[애플리케이션 시작] --> Create[현재 이벤트 루프에서 client 생성]
    Create --> Use[요청 간 재사용]
    Use --> Shutdown[종료 신호]
    Shutdown --> Close[같은 수명 경계에서 close]
```

- 앱 시작 시 연결 풀·driver를 만들고 종료 시 명시적으로 닫는다.
- 요청마다 client를 새로 만들지 않는다. connection pool을 잃고 지연·소켓 누수가 늘어난다.
- 이벤트 루프가 바뀌는 테스트, thread, 동기 wrapper 환경에서는 loop-bound 자원을 무심코 재사용하지 않는다.
- lazy initialization은 첫 사용 비용을 줄이지만, 동시 첫 호출에 대한 lock과 실패 후 재시도 정책이 필요하다.

## 이벤트 루프 귀속 문제

일부 async driver와 lock은 만든 이벤트 루프 또는 실행 context에 귀속된다. 다른 loop에서 같은 객체를 await하면 "different event loop" 오류, 끊긴 연결, 교착 상태가 날 수 있다.

```python
class LoopBoundClient:
    def __init__(self):
        self._client = None
        self._loop = None
        self._lock = None

    async def get(self):
        loop = asyncio.get_running_loop()
        if self._loop is not None and self._loop is not loop:
            raise RuntimeError("다른 이벤트 루프의 client를 재사용할 수 없습니다")
        if self._lock is None:
            self._loop = loop
            self._lock = asyncio.Lock()
        async with self._lock:
            if self._client is None:
                self._client = await create_client()
            return self._client
```

다른 loop가 필요하면 기존 client를 새 loop로 옮기지 말고, loop별 holder를 만들거나 애플리케이션 수명 경계를 하나로 통일한다. 실제 라이브러리의 thread·loop 안전성 보장은 다르므로, "client는 전역 싱글톤이면 된다"고 가정하지 말고 해당 driver 문서를 확인한다.

## timeout, cancellation, cleanup

```python
async def fetch_with_timeout(client, request):
    try:
        return await asyncio.wait_for(client.fetch(request), timeout=5)
    except asyncio.TimeoutError:
        raise RuntimeError("외부 서비스 응답 시간 초과")
```

취소는 오류가 아니라 제어 흐름일 수 있다. `CancelledError`를 무조건 삼키면 서버 종료와 사용자 취소가 늦어진다. 파일·세마포어·DB transaction은 `finally` 또는 async context manager로 정리한다.

## 동시성 경계

- `asyncio.gather`는 독립 I/O에 적합하다.
- `Semaphore`는 모델 API, DB, 웹 요청의 동시성 상한에 쓴다.
- CPU 바운드 임베딩·대형 재정렬은 event loop에서 직접 돌리지 말고 worker process나 별도 서비스로 보낸다.
- shared mutable state를 바꾸는 코루틴은 [[Concurrent Tool Execution|직렬화]]하거나 version check·transaction을 사용한다.

## 관련

- [[async-await]]
- [[Lazy Initialization]]
- [[Double-Checked Locking]]
- [[ContextVar]]
- [[Concurrent Tool Execution]]
- [[Fail-soft]]
