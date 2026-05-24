- Structured Output = [[LLM(Large Language Model)]]에게 **자유 텍스트가 아닌 정해진 스키마(JSON, Pydantic)에 맞는 출력**을 강제하는 기능.
- [[AI Agent|에이전트]]에서 [[Tool Calling]] 다음으로 자주 쓰는 기능 — 분류, 추출, 라우팅, 평가의 안정성이 크게 올라간다.

## 왜 자유 텍스트는 위험한가

```python
# 안 좋음
resp = llm.invoke("이메일에서 사람 이름과 회사명을 뽑아줘")
# 결과 예: "이름: 김철수\n회사: ABC"  ← 형식이 매번 다름, 파싱 깨짐
```

- 형식이 한 글자라도 다르면 파싱 실패.
- 사용자 LLM이 친절한 인사말을 덧붙이면 깨진다.
- production 재현성·테스트가 어렵다.

## OpenAI Structured Output

```python
from pydantic import BaseModel

class Person(BaseModel):
    name: str
    company: str

resp = client.beta.chat.completions.parse(
    model="gpt-4o-mini",
    messages=[{"role":"user", "content": text}],
    response_format=Person,
)
person: Person = resp.choices[0].message.parsed   # 이미 검증된 객체
```

- 모델이 JSON Schema를 따르도록 디코딩 단계에서 강제 → 100% 형식 보장.

## Anthropic — Tool Use로 우회

```python
# 도구 하나만 등록하고 강제 호출
tools = [{
    "name": "extract_person",
    "input_schema": Person.model_json_schema(),
    "description": "사람 정보 추출",
}]
resp = client.messages.create(
    model="claude-sonnet-4-6",
    messages=[...],
    tools=tools,
    tool_choice={"type": "tool", "name": "extract_person"},
)
person = Person(**resp.content[0].input)
```

## LangChain 통일 인터페이스

```python
from langchain_openai import ChatOpenAI

structured = ChatOpenAI(model="gpt-4o-mini").with_structured_output(Person)
person = structured.invoke(text)
```

- 공급자가 무엇이든 같은 API. [[LLM Provider 추상화]]의 한 측면.

## 활용 시나리오

| 시나리오 | 출력 스키마 |
|---------|-------------|
| 의도 분류 | `Literal["refund", "shipping", "general"]` |
| 엔티티 추출 | `list[Entity]` |
| 평가 채점 ([[LLM-as-Judge]]) | `Judgement(score: int, reason: str)` |
| 계획 ([[Planning]]) | `Plan(steps: list[str])` |
| 라우팅 결정 ([[Supervisor 패턴]]) | `Route(next: Literal["a","b","c"])` |

## JSON mode vs Structured Output

- **JSON mode** — "JSON으로만 응답해라" 라는 약한 제약. 스키마 보장 X.
- **Structured Output** — 스키마 100% 보장 (OpenAI, Vertex).
- 가능하면 항상 Structured Output 사용.

## 흔한 함정

- **enum 값에 한글** — 일부 모델은 ASCII enum이 더 안정적.
- **너무 복잡한 중첩 스키마** — 깊이 3~4 이상은 정확도가 떨어진다. 평탄화 권장.
- **Optional 필드 남발** — 모델이 비울지 채울지 헷갈림. 가능하면 default 값 부여.
- **description을 안 채움** — 필드 이름만으로 모델이 의미를 추측. `Field(description=...)`을 적자.

## 관련

- [[Pydantic]] — 스키마 정의 표준.
- [[Tool Calling]] — 내부적으로 같은 메커니즘.
- [[Intent Classification]] · [[Planning]] · [[LLM-as-Judge]] — 대표 활용처.
