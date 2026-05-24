- Prompt Engineering = [[LLM(Large Language Model)]]에게 원하는 출력을 얻기 위해 **프롬프트의 구조·내용·형식**을 설계하는 기술. 같은 모델이라도 프롬프트에 따라 정확도·비용·일관성이 크게 달라진다.
- 모델이 강해질수록 "마법의 문구"는 줄어들지만, **컨텍스트 엔지니어링**(어떤 정보를 어떻게 압축해 넣을지)의 비중은 오히려 커진다.

## 기본 구성요소

```
[Role] 당신은 ...이다
[Instruction] 다음 작업을 수행하라
[Context / Reference] 참고 문서/예시
[Constraint] 형식·길이·금지사항
[Output Format] JSON 스키마 등
[Input] 실제 사용자 입력
```

## 7가지 핵심 테크닉

### 1. Zero-shot

```text
다음 문장의 감성을 positive/negative로 분류해라.
문장: {input}
```

### 2. Few-shot

```text
예시 1) "오늘 재밌었다" → positive
예시 2) "최악이다" → negative
새 문장: "그저 그랬다" →
```

- 보통 3~8개 예시. 다양성과 난이도 분포가 중요.

### 3. Chain-of-Thought (CoT)

```text
단계별로 생각하면서 답해라.
질문: 3 + 4 * 2 = ?
```

- 추론 단계를 텍스트로 끌어내면 정확도 ↑. [[ReAct 패턴]]의 토대.

### 4. Self-Consistency

- 같은 질문을 temperature>0으로 N번 호출 → 다수결.
- CoT의 안정성을 높임.

### 5. Tree of Thoughts (ToT)

- 여러 분기를 동시에 생성·평가하고 최선을 고름.
- 복잡한 퍼즐·게임 풀이에 유효.

### 6. Role Prompting

```text
당신은 시니어 SRE다. 아래 장애 로그를 본 후, 우선순위가 가장 높은 액션을 한 줄로 답해라.
```

### 7. Structured Output

```python
# [[Pydantic]] 스키마 강제
client.beta.chat.completions.parse(..., response_format=MySchema)
```

## 좋은 프롬프트의 5가지 원칙

1. **구체적**: 모호한 형용사("좋은 답") 대신 "300자 이내, 한국어, 격식체".
2. **분리**: 지시·예시·입력을 빈 줄/구분자로 명확히 분리. `<context>...</context>` 태그.
3. **부정문보다 긍정문**: "X 하지 마라" 보다 "Y 하라".
4. **출력 형식 명시**: JSON·markdown·표 등 형식 지정.
5. **예시는 다양성**: 비슷한 예시 5개보다 다른 분포의 3개.

## 컨텍스트 엔지니어링

- 프롬프트가 길어질수록 모델이 **중간 내용을 잊는다**(lost in the middle).
- 권장: 가장 중요한 지시는 **맨 위 또는 맨 아래**, 컨텍스트는 중간.
- 정보 압축: 원본 → 요약 → 인덱스. 큰 컨텍스트 윈도우라도 무한정 넣지 말 것.

## Prompt Caching

- Anthropic, OpenAI 모두 지원. 동일한 prefix 프롬프트를 재호출하면 입력 토큰 단가가 90%↓.
- 시스템 프롬프트·few-shot 예시·도구 정의처럼 **거의 안 바뀌는 부분을 prefix로** 두는 게 핵심.

## 운영 팁

- 프롬프트는 코드처럼 **버전 관리** (`prompts/` 폴더, git).
- A/B 테스트와 [[Evaluation|회귀 평가]]를 매 변경마다.
- "프롬프트로 모든 걸 해결하려 하지 말 것" — 도구·RAG·코드 분기로 푸는 게 더 안정적인 경우가 많다.

## 관련

- [[System Prompt]] — 가장 중요한 프롬프트 종류.
- [[Structured Output]] — 출력 형식 강제.
- [[Evaluation]] — 프롬프트 변경 영향 측정.
- [[ReAct 패턴]] — CoT의 에이전트 적용.
