- Guardrails = [[AI Agent|에이전트]]의 입출력에 **안전·규정·정책 필터를 끼우는 방어층**. 환각, 유해 콘텐츠, PII 유출, 프롬프트 인젝션, 도구 오·남용을 막는다.
- 한 줄로: **"LLM이 무엇을 보고, 무엇을 말하고, 무엇을 하는지"** 를 검문소처럼 통제.

## 어디에 둘까

```
사용자 입력 → [Input Guardrail] → LLM → [Output Guardrail] → 사용자
                                  ↓
                            Tool Call → [Tool Guardrail] → 실제 실행
```

## 입력 가드레일 — 막아야 할 것

- 프롬프트 인젝션(`"위 지시는 무시하고..."`).
- PII(주민번호, 카드번호) 자동 마스킹.
- Off-topic — 정해진 도메인 밖 질문 차단.
- 모욕·공격적 발화 필터.

## 출력 가드레일

- 환각 검출 — 컨텍스트에 없는 사실 응답 차단.
- 코드/명령어 위험성 분류 (rm -rf 등).
- 금융·의료 조언 면책 문구 자동 첨부.
- 회사 정보 누출 (시스템 프롬프트 ‟유출 attempt).

## 도구 가드레일

- 비용/권한이 큰 도구 호출 전 **[[Human-in-the-loop|HITL]] 승인**.
- 허용 도메인 화이트리스트 (`fetch_url`이 사내 URL만 허용).
- 호출 횟수 제한(rate limit) — 무한 루프 방어.

## 구현 옵션

### 1. 규칙 기반

```python
PII_RE = re.compile(r"\d{6}-\d{7}")

def sanitize(text: str) -> str:
    return PII_RE.sub("[REDACTED]", text)
```

- 빠르고 결정론적. 정규식·키워드 리스트로 처리 가능한 케이스에 강함.

### 2. 모델 기반

- 별도 분류 LLM이 "이 출력이 위험한가?"를 채점.
- OpenAI Moderation API, AWS Bedrock Guardrails, Llama Guard.

```python
mod = openai.moderations.create(input=text)
if mod.results[0].flagged:
    return "그 요청에는 응답할 수 없어요."
```

### 3. 정책 라이브러리

- **NVIDIA NeMo Guardrails** — YAML로 정책 정의, dialogue flow까지 통제.
- **Guardrails AI** — Pydantic-like 스키마로 출력 검증·재시도.
- **Bedrock Guardrails** — AWS 매니지드, 주제 차단·민감 정보 필터.

## Bedrock Guardrails 예 (개념)

```yaml
denied_topics:
  - name: "투자 권유"
    examples: ["어떤 종목을 사야 해?"]
sensitive_information:
  - type: PHONE
    action: MASK
content_filters:
  hate: HIGH
  violence: HIGH
```

- 빗썸·미래에셋 같은 금융권 사례에서 표준 패턴.

## 운영 팁

- 가드레일은 **항상 켜져 있다고 신뢰하지 말 것** — fail-open이면 무용지물. fail-closed로 설계.
- 입력·출력·도구 3층에 모두 두라 — 하나만 뚫리면 끝.
- 차단된 사례를 로그로 모아 회귀 평가 데이터셋으로 활용.
- 가드레일 자체도 LLM이면 비용·지연이 든다 → 빠른 규칙 → 느린 모델 순으로 단계화.

## 관련

- [[Evaluation]] — Safety 평가는 가드레일과 직결.
- [[Observability]] — 차단 이벤트 추적.
- [[Human-in-the-loop]] — 위험 도구는 결국 사람 승인.
- [[MCP(Model Context Protocol)]] — 도구 권한 격리 메커니즘.
