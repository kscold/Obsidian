- System Prompt = 모든 사용자 발화 **이전에 LLM에게 주어지는 기본 지시문**. [[AI Agent|에이전트]]의 정체성·역할·규칙·도구 사용 방침을 정의한다.
- 사용자 입력은 자주 바뀌지만 system prompt는 거의 고정 → **prompt caching**으로 비용 절감이 가장 큰 부분.

## 좋은 system prompt 구조

```text
[정체성]
당신은 BigZami의 데이터 분석 어시스턴트다. 한국어로 답한다.

[원칙]
- 사용자가 제공한 데이터셋을 벗어난 사실을 말하지 않는다.
- 모르면 모른다고 말한다.
- 실행 가능한 도구만 호출한다.

[도구 사용]
- analyze_dataset: 데이터 분포·결측치 확인이 필요할 때.
- visualize: 시각화 요청 시.

[출력 형식]
- 코드 블록은 ```python으로.
- 표는 markdown으로.

[금지]
- 의료·법률 조언.
- 사용자의 개인정보 저장.
```

## 자주 들어가는 7가지

1. **역할 / 페르소나** — "당신은 ~다"
2. **목표** — 이 에이전트가 최종적으로 달성해야 할 것
3. **언어·톤** — 한국어·격식체 등
4. **사용 가능한 도구 설명** — 각 도구의 언제 쓸지
5. **출력 형식 규칙** — JSON·markdown
6. **거부 정책** — 어떤 요청은 거부해야 하는지
7. **메모리·컨텍스트 참조 규칙** — 사용자 정보를 어떻게 다룰지

## 모델별 차이

| 모델 | 특징 |
|------|------|
| OpenAI | `role: "system"` 메시지. 강하게 작용 |
| Anthropic Claude | 별도 `system` 파라미터. XML 태그(`<rules>`) 잘 따름 |
| Llama / Mistral | template에 따라 `<<SYS>>...<</SYS>>` |
| Gemini | `systemInstruction` 필드 |

## 흔한 함정

- **충돌하는 지시** — "친근하게" + "격식 있게" 같이 모순. 우선순위 명시 필요.
- **너무 긴 system prompt** — 5K 토큰을 넘기면 중간 지시를 잊는다. 핵심을 끝부분에 반복.
- **부정 표현 남발** — "X 하지 마라"보다 긍정 표현이 더 잘 지켜진다.
- **프롬프트로 모든 걸 통제하려 함** — 코드 게이트·[[Guardrails|가드레일]]·[[Tool Calling|도구 분기]]가 더 안정적인 경우가 많다.

## 외재화(externalization)

- 프롬프트를 코드 문자열로 박지 말고 `prompts/system.md` 같은 별도 파일로 분리:
  - git diff가 깔끔해 변경 이력 추적.
  - 비개발자도 PR 가능.
  - 모델/언어별로 폴더 구분 가능.
  - A/B 테스트가 쉽다.

```python
from pathlib import Path
SYSTEM_PROMPT = Path("prompts/system.md").read_text()
```

## Prompt Caching 활용

- system prompt를 **첫 1024+ 토큰**으로 고정 → Anthropic·OpenAI cache hit.
- 사용자 발화는 그 뒤에 붙어 입력 단가 90%↓.

```python
# Anthropic 예
messages.create(
    system=[{"type": "text", "text": SYSTEM_PROMPT,
             "cache_control": {"type": "ephemeral"}}],
    messages=[{"role": "user", "content": user_input}],
)
```

## 관련

- [[Prompt Engineering]] — 상위 기법.
- [[LLM Provider 추상화]] — 모델별 system 처리를 추상화.
- [[Tool Calling]] — 도구 설명이 보통 system에 들어감.
