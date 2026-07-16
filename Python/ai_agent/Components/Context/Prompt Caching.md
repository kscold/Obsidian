---
title: Prompt Caching
created: 2026-07-15
tags:
  - ai-agent
  - llm
  - context-engineering
  - cost
---

# Prompt Caching

Prompt Caching은 매 호출마다 반복되는 긴 프롬프트 앞부분을 공급자 캐시에 재사용해 **입력 토큰 비용과 첫 응답 지연을 줄이는 기법**이다. 답변을 재사용하는 캐시가 아니라, 모델이 이미 처리한 안정적인 컨텍스트를 재사용하는 캐시다.

## 무엇과 다른가

| 종류 | 재사용 대상 | 주의점 |
|---|---|---|
| Prompt cache | 동일한 프롬프트 prefix의 처리 결과 | prefix와 보안 경계가 같아야 한다. |
| Semantic cache | 의미가 비슷한 질문의 최종 답변 | 오래된 답 또는 잘못된 답을 재사용할 위험이 있다. |
| Tool result cache | 검색, DB 조회, 계산 결과 | 데이터 최신성, 권한, TTL을 따로 관리한다. |

Prompt cache는 사용자 질문이 조금 달라도 안정적인 앞부분이 같으면 이득을 얻는다. 따라서 같은 시스템 프롬프트, 도구 스키마, 정책, 예시를 계속 보내는 Agent에서 특히 효과가 크다.

## Prefix와 Suffix를 분리한다

~~~mermaid
flowchart LR
    Prefix["안정 prefix<br/>시스템 지시 / 정책 / 도구 스키마 / 버전 고정 예시"]
    Suffix["변동 suffix<br/>현재 요청 / 최신 상태 / 도구 결과 / 사용자별 정보"]
    Prefix --> Cache["Provider prompt cache"]
    Suffix --> Call["현재 LLM 호출"]
    Cache --> Call
~~~

- **안정 prefix**: 긴 시스템 지시, 변하지 않는 규칙, 공통 도구 설명, 고정된 few-shot 예시를 앞에 둔다.
- **변동 suffix**: 현재 요청, 현재 세션 상태, 방금 받은 도구 결과, 실시간 데이터를 뒤에 둔다.
- 캐시를 위해 정보를 억지로 고정하면 안 된다. 최신 권한, 정책, 사용자의 요청은 항상 현재 값이어야 한다.

## 캐시 키 설계

공급자마다 캐시 활성화 방식은 다르지만, 애플리케이션이 키를 만들 때는 다음 식별자를 안정적으로 포함하는 편이 좋다.

~~~text
agent : prompt-cache : prompt-version : scope : model : capability-set
~~~

예를 들어 planner, intent-classifier, specialist처럼 호출 목적을 scope로 두고, 프롬프트나 도구 스키마가 바뀔 때 prompt-version을 올린다.

- 사용자 원문, API 키, 개인정보를 캐시 키에 넣지 않는다.
- 테넌트·권한이 다른 컨텍스트는 같은 키를 공유하지 않는다.
- 모델이나 tool schema가 달라지면 같은 prefix처럼 보여도 별도 키로 본다.
- 키는 **응답 캐시 키**가 아니다. 최신 답을 보장해야 하는 호출은 매번 모델을 실행한다.

## 무효화 기준

다음 중 하나가 바뀌면 기존 prefix를 재사용하지 않도록 버전을 나눈다.

1. 시스템 정책, 금지 규칙, 출력 스키마
2. 도구 이름, 인자 스키마, 권한 설명
3. 모델 또는 tokenizer 계열
4. 테넌트, 역할, 데이터 접근 범위
5. few-shot 예시와 기준 지식

시간 기반 TTL만으로는 부족하다. 프롬프트 의미가 달라졌는지가 더 중요한 무효화 조건이다.

## 관측해야 할 지표

- 전체 입력 토큰과 cache read 토큰
- cache hit 비율과 prefix별 miss 비율
- cache hit 시 첫 토큰 지연 시간과 비용
- 프롬프트 버전별 비용 변화
- 캐시 적용 전후의 품질 차이

캐시 적중률만 높고 답변 품질이나 보안이 나빠지면 성공이 아니다. [[Cost와 Token]]과 [[Observability]]에서 비용·지연·오류를 같은 trace로 보며, [[Context Engineering]]에서 prefix 자체가 현재 판단에 정말 필요한지도 계속 줄인다.

## 실무 체크

- 시스템 프롬프트와 tool schema의 순서를 고정한다.
- 긴 정적 문서는 캐시 가능하더라도 필요한 부분만 넣는다.
- 현재 사용자 요청과 실시간 도구 결과는 cacheable prefix에 섞지 않는다.
- provider별 cache metadata가 없을 때는 적중을 추정하지 말고 unknown으로 기록한다.
- 캐시가 비활성화되어도 기능이 정상 동작하도록 설계한다.

## 관련

- [[Context Engineering]]
- [[LLM Routing]]
- [[LLM Provider 추상화]]
- [[Cost와 Token]]
- [[Observability]]
