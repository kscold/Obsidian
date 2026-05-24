- Hierarchical Agent = [[Supervisor 패턴]]을 **여러 층으로 쌓은** [[Multi Agent|멀티 에이전트]] 트리. 최상위 supervisor가 중간 supervisor들에게 작업을 위임하고, 중간 supervisor가 다시 워커들에게 위임.
- 큰 조직도(CEO → 부서장 → 팀원)와 같은 구조 — 작업과 권한이 계층적으로 분해된다.

## 언제 필요한가

- 단층 [[Supervisor 패턴]]만으로는 워커가 너무 많아 라우팅 정확도가 떨어질 때 (대략 10명 이상).
- 도메인이 명확히 부서로 나뉠 때 (예: 영업팀 / 개발팀 / 운영팀).

## 구조 예

```
                    Top Supervisor
                   /              \
            Research Lead     Engineering Lead
           /     |      \      /     |      \
        web    paper  legal  coder  tester  reviewer
```

## LangGraph 구현 아이디어

```python
# 각 leaf는 그 자체로 작은 그래프(서브그래프)
research_subgraph = build_research_supervisor()
eng_subgraph = build_eng_supervisor()

top = StateGraph(State)
top.add_node("research", research_subgraph)   # 서브그래프를 노드로 임베드
top.add_node("engineering", eng_subgraph)
top.add_node("top_supervisor", top_router)
top.add_conditional_edges("top_supervisor", route, {
    "research": "research",
    "engineering": "engineering",
    "FINISH": END,
})
```

## 장점

- 책임 분해가 명확 — 각 supervisor의 의사결정 범위가 좁아 라우팅 정확도 ↑.
- 부분 시스템을 독립적으로 개발·테스트할 수 있다 (서브그래프).

## 단점

- 메시지 패싱 단계가 늘어나 **지연·비용 증가**.
- 깊은 호출 스택은 디버깅이 까다롭다.
- 잘못 설계하면 사용자가 던진 작은 요청이 여러 계층을 거치며 폭주.

## 권장 팁

- 깊이 2~3층을 넘기지 말 것.
- 각 supervisor에 `max_turns`를 두고, 부모로 빠르게 escalate.
- 서브그래프의 입출력 스키마(State 일부분)를 명시적으로 잡아둘 것.

## 관련

- [[Supervisor 패턴]] — 1층 버전.
- [[Multi Agent]] · [[Swarm 패턴]].
