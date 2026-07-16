---
title: LLM Routing
created: 2026-07-15
tags:
  - ai-agent
  - llm
  - routing
  - runtime
---

# LLM Routing

LLM Routing은 요청마다 어떤 모델·공급자·호출 구성을 사용할지 결정하는 정책이다. [[LLM Provider 추상화]]가 "어떻게 같은 인터페이스로 호출할지"를 해결한다면, routing은 "**이번 작업에 어떤 모델이 적합한가**"를 결정한다.

## 라우팅은 제약을 먼저 본다

~~~mermaid
flowchart LR
    Request["요청과 실행 상태"] --> Constraints["필수 제약 필터"]
    Constraints --> Candidates["호환 모델 후보"]
    Candidates --> Score["품질 / 지연 / 비용 점수"]
    Score --> Selected["선택 모델"]
    Selected --> Observe["결과 관측"]
    Observe --> Fallback["실패 시 fallback 정책"]
~~~

점수 계산 전에 불가능한 후보를 제외한다.

| 제약 | 질문 |
|---|---|
| Capability | tool calling, structured output, vision, streaming이 필요한가? |
| Context | 필요한 입력과 출력이 모델 context window 안에 들어가는가? |
| Security | 데이터 지역, 테넌트, 승인된 공급자 조건을 만족하는가? |
| Reliability | 현재 rate limit, 장애, timeout 예산에서 실행 가능한가? |
| Cost | 호출 예산과 호출 횟수 제한 안에 드는가? |

그 다음에만 품질, 예상 지연, 비용, 캐시 적중 가능성을 비교한다.

## 대표 라우팅 단계

1. **정적 정책**: 분류, 요약, 복잡한 추론처럼 안정적인 작업 유형별 기본 모델을 정한다.
2. **런타임 선택**: 입력 길이, 도구 필요 여부, 사용자 등급, 부하, 공급자 상태로 후보를 좁힌다.
3. **호출 구성**: 선택한 모델에 맞는 tools, structured output, cache key, timeout을 새로 조립한다.
4. **Fallback**: 재시도가 의미 있는 오류에만 대체 모델이나 대체 공급자를 사용한다.
5. **평가 피드백**: routing 결정과 결과 품질을 저장해 정책을 조정한다.

## 호환성은 모델 이름보다 중요하다

~~~python
def select_model(request, candidates):
    compatible = [
        model for model in candidates
        if model.supports_tools >= request.needs_tools
        and model.supports_structured_output >= request.needs_schema
        and model.max_context >= request.token_budget
        and model.allowed_for(request.data_classification)
    ]
    return min(compatible, key=lambda model: model.expected_cost(request))
~~~

실제 정책에는 품질과 지연 가중치가 더 필요하지만, 핵심은 **호환성 필터가 비용 최적화보다 먼저**라는 점이다. tool calling 또는 structured output이 필요한 요청에 호환되지 않는 모델을 보내고 fallback하는 것은 routing이 아니라 실패를 예약하는 일이다.

선택 뒤에는 모델에 맞는 Runnable을 새로 구성한다. 이전에 tools나 출력 스키마가 묶인 객체를 다른 모델 선택 결과에 재사용하면 모델 능력·스키마·캐시 키가 어긋날 수 있다.

## Fallback과 회로 차단

- 429, 일시적 네트워크 오류, 공급자 장애는 제한된 재시도 또는 대체 후보가 의미 있을 수 있다.
- 입력 정책 위반, 지원하지 않는 capability, 잘못된 tool schema는 fallback하지 말고 즉시 실패해야 한다.
- 연속 장애 공급자는 circuit breaker로 잠시 후보에서 제외한다.
- fallback이 품질이나 기능을 낮추면 사용자·운영자에게 그 사실을 남긴다.

## 평가 지표

- 모델별 성공률, 구조화 출력 유효율, tool call 오류율
- task 유형별 품질 점수와 사람 수정 비율
- p50/p95 지연, 입력·출력·cache 토큰 비용
- fallback 비율과 fallback 뒤 성공률
- 테넌트·언어·길이 구간별 편향

라우팅은 "가장 싼 모델 고르기"가 아니다. [[Evaluation]] 결과로 품질 하한을 정하고, 그 하한 안에서 비용과 지연을 최적화하는 운영 정책이다.

## 관련

- [[LLM Provider 추상화]]
- [[Prompt Caching]]
- [[Structured Output]]
- [[Tool Calling]]
- [[Fallback]]
- [[Observability]]

## 공식 문서

- [LangChain 모델 선택과 동적 모델](https://docs.langchain.com/oss/python/langchain/models)
