- Agentic Loop = [[AI Agent|에이전트]]의 **심장** — `사용자 입력 → LLM 추론 → 도구 호출 → 결과 관찰 → 다시 LLM` 사이클을 종료 조건이 만족될 때까지 반복하는 핵심 구조.
- [[Strands Agents|Strands]] 표현이 가장 명료: "에이전트는 그냥 do-while 루프 안의 LLM이다."

## 의사 코드

```python
def agentic_loop(user_input: str, tools: list, max_iters: int = 25):
    messages = [system_prompt, {"role": "user", "content": user_input}]
    for _ in range(max_iters):
        # 1) LLM 호출 — 다음 행동 결정
        resp = llm.invoke(messages, tools=tools)

        # 2) 종료 조건 — 더 호출할 도구가 없음
        if not resp.tool_calls:
            return resp.content

        # 3) 도구 실행 (병렬 가능)
        results = [tools[tc.name].run(tc.args) for tc in resp.tool_calls]

        # 4) 결과를 다음 입력으로 누적
        messages.append(resp)
        messages.extend(results)
```

## 종료 조건 5가지

1. LLM이 도구를 더 호출하지 않음 (자연 종료).
2. 명시적 종료 도구 호출 (`finalize()` / `stop`).
3. `max_iters` 도달 (안전장치).
4. 비용·시간 한도 초과.
5. [[Human-in-the-loop|사용자 중단]].

## 한 턴 안에서의 사고 깊이

- **Lightweight** — 1~2회 LLM 호출, 도구 1~2개. 빠른 응답형.
- **Heavyweight** — 10~25회 반복, 도구 다양. 복잡한 분석·자동화.
- 같은 에이전트라도 의도에 따라 깊이를 다르게 가는 게 효율적 ([[Intent Classification]] → 라우팅).

## [[ReAct 패턴]]과의 관계

- ReAct는 agentic loop를 **텍스트(Thought/Action/Observation)** 로 표현한 것.
- 현대 모델은 네이티브 [[Tool Calling]]을 쓰므로 텍스트 파싱 없이 같은 루프를 돈다.

## [[LangGraph]] 구현

```python
# call_llm → should_continue → call_tools → call_llm 의 사이클
graph.add_edge("tools", "llm")
graph.add_conditional_edges("llm", should_continue, {"tools": "tools", END: END})
```

## 흔한 실패

- 종료 조건이 약해 **무한 루프** — 같은 도구를 반복 호출. 방어: 최근 N턴 동일 호출 감지.
- 누적 컨텍스트 폭주 — [[Memory|메모리]] 트리밍·요약 필수.
- 한 턴에 너무 많은 결정을 시키려 함 — [[Plan-and-Execute|계획 분리]]로 풀자.

## 관련

- [[AI Agent란]] · [[ReAct 패턴]] — 루프의 기본 형태.
- [[Tool Calling]] · [[Memory]] · [[Planning]] — 루프 안의 구성요소.
- [[Self-Improving Agent]] — 루프가 스스로를 갱신하는 확장.
