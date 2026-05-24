- LLM Provider 추상화 = **여러 LLM 공급자(OpenAI, Anthropic, Bedrock, Gemini, Ollama, vLLM 등) 를 단일 인터페이스로 감싸**서, 호출부 코드를 모델 교체에 무관하게 만드는 패턴.
- 운영 중 발생하는 가장 잦은 변경 — 모델 업그레이드/다운그레이드, 비용 절감을 위한 라우팅, 멀티 리전 fallback — 을 코드 한 줄 수정으로 끝내기 위한 layer.

## 왜 필요한가

- 공급자마다 SDK·메시지 포맷·tool call 스키마·streaming 형식이 다르다.
- 호출부가 `openai.chat.completions.create(...)`를 직접 부르면 **N곳에 흩어진 SDK 호출**이 모델 교체 시 모두 수정 대상이 된다.
- 추상화하면 모델 ID만 바꾸면 끝.

## Factory 패턴

```python
from abc import ABC, abstractmethod
from typing import Iterable

class LLMProvider(ABC):
    @abstractmethod
    def invoke(self, messages: list, **kwargs) -> str: ...
    @abstractmethod
    def stream(self, messages: list, **kwargs) -> Iterable[str]: ...
    @abstractmethod
    def with_tools(self, tools: list) -> "LLMProvider": ...

class OpenAIProvider(LLMProvider): ...
class AnthropicProvider(LLMProvider): ...
class BedrockProvider(LLMProvider): ...

def llm_factory(spec: str) -> LLMProvider:
    """spec 예: 'openai/gpt-4o-mini', 'bedrock/anthropic.claude-3-5-sonnet'"""
    vendor, model = spec.split("/", 1)
    return {
        "openai": OpenAIProvider,
        "anthropic": AnthropicProvider,
        "bedrock": BedrockProvider,
    }[vendor](model)
```

- 호출부는 `llm = llm_factory(settings.LLM)` 한 줄.

## 이미 잘 만들어진 라이브러리

- **LiteLLM** — 100+ 공급자 통합. OpenAI 포맷으로 호출하면 어떤 모델이든.
- **LangChain `init_chat_model`** — `init_chat_model("openai:gpt-4o-mini")` 형태.
- **AWS Bedrock Converse API** — Bedrock 내부 모델은 동일 API로.

```python
from litellm import completion
res = completion(model="anthropic/claude-sonnet-4-6",
                 messages=[{"role": "user", "content": "hi"}])
```

## 추상화에 꼭 포함해야 할 것

1. **invoke / stream / batch** — 3가지 호출 모드.
2. **Tool calling** — JSON Schema 일관 변환.
3. **Structured output** — [[Pydantic]] 모델 직접 지원.
4. **Usage 통계** — input/output 토큰, cached 토큰을 표준화.
5. **에러 분류** — 401(인증) / 429(rate) / 500(공급자) / context_length 등 표준 예외.
6. **로깅·트레이싱 훅** — [[Observability]] 후크.

## Routing — 한 단계 위

```python
def smart_route(task_type: str) -> LLMProvider:
    if task_type == "classification": return llm_factory("openai/gpt-4o-mini")
    if task_type == "reasoning":      return llm_factory("anthropic/claude-sonnet-4-6")
    if task_type == "coding":         return llm_factory("openai/o3")
```

- 작업 유형에 따라 다른 모델 — 비용 70% 절감도 가능.

## Fallback

```python
for provider_spec in PRIMARY, SECONDARY, TERTIARY:
    try: return llm_factory(provider_spec).invoke(msgs)
    except (RateLimitError, ProviderDown):
        continue
```

- Bedrock·OpenAI 다중 리전 / 다중 공급자 가용성 보호.

## LLM Gateway (운영 확장)

- 사내 LLM 게이트웨이 = **추상화 + 인증 + 비용 한도 + 감사 + 모델 라우팅**을 별도 서비스로.
- 빗썸·미래에셋 사례: 모든 LLM 호출이 Gateway를 통과해 SIEM·감사로그·예산 통제.

## 관련

- [[LLM(Large Language Model)]] — 추상화의 대상.
- [[Tool Calling]] — 공급자별 스키마 차이 흡수.
- [[Observability]] · [[Cost와 Token]] — 게이트웨이의 핵심 부가가치.
- [[Guardrails]] — Gateway의 보안 책임.
