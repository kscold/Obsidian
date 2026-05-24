- LLM 비용은 거의 100% **토큰 사용량**에 비례한다. [[AI Agent|에이전트]] 운영에서 가장 빠르게 폭주하는 변수이자 가장 큰 최적화 포인트.
- 비용 = `input_tokens × input_price + output_tokens × output_price` (대개 출력 단가가 입력의 3~5배).

## 토큰이란

- 모델이 입력·출력을 처리하는 최소 단위.
- 영어: 1단어 ≈ 1.3 토큰, 한국어: 1글자 ≈ 1~3 토큰 (모델 토크나이저별 차이).
- 컨텍스트 윈도우 = 입력+출력 합산 한도 (GPT-4o 128K, Claude 200K, Gemini 1M+).

## 토큰 카운팅

```python
# OpenAI
import tiktoken
enc = tiktoken.encoding_for_model("gpt-4o-mini")
n = len(enc.encode(text))

# Anthropic
from anthropic import Anthropic
n = client.count_tokens(text)
```

## 응답에서 usage 추출

```python
resp = client.chat.completions.create(...)
u = resp.usage
# u.prompt_tokens, u.completion_tokens, u.total_tokens
# u.prompt_tokens_details.cached_tokens  ← prompt cache hit
```

- 모든 호출의 usage를 [[Observability|트레이스]]에 저장 → 사용자/엔드포인트별 비용 분석.

## 비용 절감 기법 7가지

### 1. Prompt Caching

- Anthropic·OpenAI 모두 지원. 시스템 프롬프트·도구 정의·few-shot 같은 **거의 안 바뀌는 prefix**를 캐싱하면 입력 단가 **90%↓**.

### 2. 모델 라우팅

- 분류·간단 작업엔 `gpt-4o-mini`, 복잡 추론에만 `gpt-4o` / `o3`. [[LLM Provider 추상화]] 참조.

### 3. 출력 길이 제한

- `max_tokens` 명시 + 시스템 프롬프트에서 "300자 이내"라 못박기.

### 4. 컨텍스트 트리밍

```python
from langchain_core.messages import trim_messages
trimmed = trim_messages(messages, max_tokens=4000, strategy="last")
```

### 5. RAG로 대체

- 모든 문서를 컨텍스트에 넣지 말고 [[RAG(Retrieval-Augmented Generation)|검색]]으로 필요한 청크만.

### 6. Batch API

- OpenAI/Anthropic 둘 다 비실시간 batch 호출에 **50% 할인**. 평가·대량 분류에 적합.

### 7. Structured Output / Tool Calling

- 자유 텍스트로 길게 받은 뒤 파싱하지 말고, 처음부터 [[Structured Output|구조화 출력]]으로 짧게.

## 출력 단가가 비싸다 — 짧게

- gpt-4o 기준 입력 $2.5 / 1M, 출력 $10 / 1M.
- "왜 그렇게 답했는지 길게 설명해" 가 가장 비싼 요청.
- CoT 추론도 보이는 출력으로 빼면 토큰이 곱하기 — 가능하면 도구 호출이나 hidden reasoning 모델(o-시리즈)로.

## 예산 통제

- 사용자/엔드포인트별 일·월 한도. 초과 시 fail-closed.
- [[Observability]] 대시보드에 비용 알람(예: 일 예산 80% 도달 시 슬랙).
- 사내 [[LLM Provider 추상화|LLM Gateway]]가 이 통제의 자연스러운 자리.

## 관련

- [[Observability]] · [[Evaluation]] — 비용도 KPI.
- [[LLM Provider 추상화]] — 라우팅 / 게이트웨이.
- [[Prompt Engineering]] · [[Memory]] — 입력 토큰 최소화.
