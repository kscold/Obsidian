---
title: LLM 샘플링 파라미터
created: 2026-08-28
tags:
  - ai-agent
  - llm
  - runtime
---

# LLM 샘플링 파라미터

LLM 호출에서 실제로 손대는 인자들. **이름이 provider마다 다르다**는 게 첫 함정이다.

| 의미 | OpenAI | [[Ollama]] |
|---|---|---|
| 생성 토큰 상한 | `max_tokens` | `num_predict` |
| 컨텍스트 창 | (모델 고정) | `num_ctx` |
| 무작위성 | `temperature` | `temperature` |
| 호출 상한 시간 | `timeout` | `request_timeout` |

```python
kwargs = {
    "model": model_name,
    "temperature": temperature,
    "num_predict": max_tokens,          # 호출부는 max_tokens 라는 한 이름만 안다
    "num_ctx": settings.ollama_num_ctx,
}
```

빌더가 **이름 차이를 흡수**한다. 호출부가 provider를 알면 [[LLM Provider 추상화]]가 깨진다.

## temperature

- 0에 가까울수록 **같은 입력에 같은 답**. 1에 가까울수록 다양해진다.
- 분류·추출·계획처럼 **답이 정해진 작업은 0 근처**가 맞다. [[Structured Output]]과 함께 쓰면 재현성이 크게 오른다.
- 0이어도 완전한 결정성은 아니다. 배치·하드웨어·부동소수점 순서 때문에 미세하게 흔들린다. **테스트를 정확 일치로 짜면 깨진다.**

## num_predict / max_tokens

- **생성분의 상한**이다. 컨텍스트 창에서 이만큼을 미리 떼어 놓는 셈 ([[컨텍스트 윈도우와 num_ctx]]).
- 너무 작으면 답이 잘린다(`done_reason=length`). 너무 크면 폭주하는 모델이 창을 다 먹는다.
- 긴 JSON을 요구하는 [[Structured Output]] 호출에서는 특히 넉넉해야 한다. 잘린 JSON은 복구가 불가능하다.

## timeout — 상한의 주인은 하나

```python
if request_timeout:
    kwargs["timeout"] = request_timeout
```

> request_timeout은 단일 LLM 호출 상한의 정본을 호출자가 전달한다 — client 자체 기본값에 의존하면 **상한 소유자가 둘이 된다.**

라이브러리 기본값에 맡기면 "어디서 정한 값인지" 아무도 모르게 된다. 설정 하나가 정본이어야 한다.

## streaming

```python
"streaming": True
```

토큰을 만들어지는 대로 흘려보낸다 ([[Streaming]], [[SSE 기반 Agent Streaming]]). 체감 지연이 크게 줄지만, **structured output과 같이 쓰면 다 받아야 파싱이 되므로** 이점이 줄어든다.

## prompt_cache_key (OpenAI 전용)

```python
if prompt_cache_key:
    kwargs["model_kwargs"] = {"prompt_cache_key": prompt_cache_key}
```

같은 접두 프롬프트를 재사용할 때 비용·지연을 줄인다 ([[Prompt Caching]]). Ollama에는 없는 개념이라 provider 분기 안에서만 붙인다.

## 한 줄 정리

파라미터의 **의미는 같고 이름은 다르다.** 빌더 한 곳에서 흡수하고, 상한값의 정본은 설정 하나가 소유한다.

## 관련

- [[Ollama]]
- [[컨텍스트 윈도우와 num_ctx]]
- [[LLM Provider 추상화]]
- [[Structured Output]]
- [[Streaming]]
- [[Prompt Caching]]
- [[Cost와 Token]]
- [[sLLM(소형 LLM) 운용]]
