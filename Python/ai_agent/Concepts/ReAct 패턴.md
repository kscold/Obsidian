- ReAct = **Reasoning + Acting**. [[LLM(Large Language Model)]]이 매 턴 "생각(Thought) → 행동(Action) → 관찰(Observation)" 사이클을 명시적으로 출력하면서 [[AI Agent|에이전트]]를 동작시키는 가장 기초적이고 많이 쓰이는 패턴이다.
- 2022년 Yao et al. 논문에서 제안. 핵심 아이디어: **추론 흔적(reasoning trace)을 LLM이 직접 텍스트로 쓰게 하면**, 도구 호출 정확도와 디버깅 가능성이 모두 올라간다.

## 작동 방식

```
Thought: 사용자가 오늘 서울 기온을 물었다. weather_tool을 써야 한다.
Action: weather_tool(city="서울")
Observation: 18°C, 흐림
Thought: 정보를 받았으니 답하면 된다.
Final Answer: 오늘 서울은 18°C, 흐림입니다.
```

## Python 의사 구현

```python
def react_loop(question: str, tools: dict, max_steps: int = 5):
    scratchpad = ""
    for _ in range(max_steps):
        prompt = REACT_TEMPLATE.format(
            question=question,
            scratchpad=scratchpad,
            tool_names=list(tools.keys()),
        )
        response = llm.invoke(prompt)

        if "Final Answer:" in response:
            return response.split("Final Answer:")[-1].strip()

        # Action: tool_name(args) 형태 파싱
        action_name, args = parse_action(response)
        observation = tools[action_name].run(args)

        scratchpad += f"{response}\nObservation: {observation}\n"
    raise RuntimeError("max_steps 초과")
```

## 프롬프트 템플릿 (전형적)

```text
다음 도구를 사용할 수 있다: {tool_names}

다음 형식을 반드시 따라라:
Thought: (다음에 할 일에 대한 생각)
Action: 도구이름(인자)
Observation: (도구 실행 결과 — 시스템이 채움)
... (반복) ...
Thought: 이제 답할 수 있다
Final Answer: (최종 답변)

Question: {question}
{scratchpad}
```

## 장점

- **해석 가능성** — Thought가 그대로 로그로 남아 왜 그 도구를 불렀는지 추적 가능.
- **에러 회복** — 도구 실패 시 Observation에 에러 메시지를 넣고 LLM이 재시도/대안 선택을 할 수 있다.
- **구현 단순** — 별도 그래프 엔진 없이 텍스트 파싱만으로 동작.

## 한계

- 단일 LLM이 모든 단계를 처리해 **긴 작업에서 컨텍스트가 누적되어 비용·지연·환각이 증가.**
- 파싱 실패에 취약 (LLM이 형식을 어기면 깨짐) — 그래서 요즘은 [[Tool Calling|구조화된 tool call]]로 대체되는 추세.
- 복잡한 분기·병렬은 표현이 어렵다 → [[LangGraph]]나 [[Plan-and-Execute]]로 확장.

## 관련 패턴

- [[Plan-and-Execute]] — 먼저 전체 계획을 세우고 실행. ReAct는 step-by-step.
- [[Reflexion]] — ReAct에 자기비판(self-critique) 루프를 추가.
- [[Tool Calling]] — 최신 LLM은 ReAct 텍스트 파싱 대신 네이티브 tool call로 같은 일을 수행.
