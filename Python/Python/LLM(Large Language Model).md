- LLM(Large Language Model) = 수십억~수조 개 파라미터를 가진 트랜스포머 기반 언어 모델. 입력 토큰 시퀀스 → 다음 토큰의 확률 분포를 예측하는 것이 본질.
- 대표 모델: OpenAI GPT-4o / o3, Anthropic Claude 3.5 Sonnet / 4, Google Gemini, Meta Llama, Mistral.
- LLM 자체는 stateless하고 학습 시점 정보만 안다 — 실시간 정보·외부 시스템과의 상호작용은 [[AI Agent|에이전트]] 인프라가 보탠다.

## 핵심 개념

### Token

- 모델이 보는 최소 단위. 영어는 보통 1 단어 ≈ 1.3 토큰, 한국어는 1 글자 ≈ 1~3 토큰.
- 입력+출력 합쳐 **컨텍스트 윈도우** 한도 안에 들어가야 함 (GPT-4o 128K, Claude 200K, Gemini 1M+).

### Temperature / Top-p

- `temperature` — 0이면 결정론적(가장 확률 높은 토큰), 1이면 다양성↑.
- 코드·도구 호출엔 0~0.3, 창작엔 0.7~1.0.

### System / User / Assistant

```python
messages = [
    {"role": "system", "content": "너는 친절한 어시스턴트다."},
    {"role": "user", "content": "안녕"},
    {"role": "assistant", "content": "안녕하세요!"},
]
```

- system은 모델의 "성격"과 규칙. user/assistant가 번갈아 가며 대화.

## Python에서 호출

```python
# OpenAI
from openai import OpenAI
client = OpenAI()
resp = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": "안녕"}],
    temperature=0.3,
)
print(resp.choices[0].message.content)

# Anthropic
from anthropic import Anthropic
client = Anthropic()
resp = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    messages=[{"role": "user", "content": "안녕"}],
)
print(resp.content[0].text)
```

## 비용 구조

- **입력 토큰 단가 < 출력 토큰 단가** (대개 4~5배 차이).
- 같은 프롬프트를 자주 보낸다면 **prompt caching**(Anthropic, OpenAI 모두 지원)으로 입력 단가 90%↓.

## 한계와 우회

| 한계 | 우회 |
|------|------|
| 학습 시점 이후 정보 부재 | [[RAG(Retrieval-Augmented Generation)]], [[Tool Calling]] |
| 환각(Hallucination) | RAG, 검증 도구, [[Reflection]] |
| 컨텍스트 윈도우 한도 | 요약, [[청킹(Chunking)]], [[Memory]] |
| 도구 사용 불가 | [[Tool Calling]] |
| 비결정성 | temperature=0, structured output |

## Structured Output

```python
from pydantic import BaseModel

class WeatherReport(BaseModel):
    city: str
    temperature: float
    condition: str

resp = client.beta.chat.completions.parse(
    model="gpt-4o-mini",
    messages=[...],
    response_format=WeatherReport,
)
report: WeatherReport = resp.choices[0].message.parsed
```

- 자유 텍스트 대신 [[Pydantic]] 스키마를 강제 → 후속 코드에서 안정적 파싱.

## 관련

- [[AI Agent]] — LLM을 두뇌로 쓰는 시스템.
- [[Tool Calling]] · [[RAG(Retrieval-Augmented Generation)]] · [[Memory]].
