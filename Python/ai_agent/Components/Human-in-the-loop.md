- Human-in-the-loop(HITL) = [[AI Agent|에이전트]] 실행 중간에 **사람의 승인·수정·중단**을 받는 절차. 부작용이 크거나 비가역적인 행동(결제, 메일 발송, DB 삭제) 직전에 끼워 넣는다.
- 자율성을 일부 포기하는 대신 **신뢰성과 안전성**을 얻는다 — production 에이전트에서 사실상 필수.

## 언제 끼울까

- **부작용이 큰 [[Tool Calling|도구]] 직전** — 결제 승인, 외부 API 호출.
- **비용이 큰 경로 직전** — 강력 모델 호출, 대량 처리.
- **모호한 입력** — 사용자 의도가 불확실할 때.
- **계획 검토** — Plan-and-Execute에서 사람이 plan을 한번 확인.

## 패턴

### 1. Interrupt (그래프 중단)

```python
# LangGraph
from langgraph.types import interrupt

def need_approval(state):
    decision = interrupt({"question": "이 결제를 진행할까요?", "amount": state["amount"]})
    return {"approved": decision}
```

- `interrupt`가 호출되면 그래프가 **그 자리에서 멈추고 상태를 저장**, 사용자 응답을 받으면 같은 thread로 resume.

### 2. Tool 안에 승인 게이트

```python
@tool
def send_email(to: str, body: str) -> str:
    """메일을 보낸다."""
    if not confirm_with_user(to, body):
        return "사용자가 거부함"
    return mailer.send(to, body)
```

### 3. Dry-run 후 확인

- 도구가 먼저 "이런 일을 하려고 한다"의 미리보기를 반환 → 다음 턴에 사용자 확인 → 실제 실행.

## 구현 시 고려사항

- **Checkpointer 필수** — 사람이 응답하기까지 시간이 걸리므로 그래프 상태가 영속화돼야 한다 (LangGraph: `SqliteSaver`, `PostgresSaver`).
- `interrupt`는 그래프를 멈추는 기능이고, [[LangGraph Checkpointer]]는 멈춘 지점을 다시 찾게 해주는 저장 장치다.
- **타임아웃** — 영원히 기다리면 자원이 묶인다.
- **컨텍스트 제공** — 사람이 판단할 수 있도록 결정의 근거를 같이 보여줄 것.
- **취소·수정 옵션** — 단순 yes/no가 아니라 "수정해서 진행"도 가능하게.

## UX 패턴

- Slack/Discord bot으로 승인 요청 → 버튼으로 응답.
- 웹 대시보드에서 pending approval 목록.
- 이메일 승인 링크.

## 관련

- [[LangGraph]] — `interrupt`, checkpoint 기반 HITL 표준.
- [[Tool Calling]] — 부작용 큰 도구에 게이트 추가.
- [[Memory]] — 사람의 수정 이력을 다음 결정에 반영.
