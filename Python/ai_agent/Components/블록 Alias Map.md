---
title: 블록 Alias Map
created: 2026-06-26
tags:
  - ai-agent
  - workflow
  - graph
  - frontend
---

# 블록 Alias Map

블록 Alias Map은 AI 응답에 들어있는 논리적 블록 이름을 실제 캔버스 blockId로 연결하는 매핑이다.

AI는 `tdl`, `source`, `model`, `preprocess_1`처럼 사람이 이해하기 쉬운 이름을 쓰고, 프론트엔드는 이를 실제 노드 ID로 바꿔서 링크와 옵션을 적용한다.

## 왜 필요한가

- 캔버스 blockId는 런타임에 생성된다.
- AI 서버는 프론트 내부 ID를 알 수 없다.
- 링크는 "출발 블록 -> 도착 블록"을 정확히 찾아야 한다.
- 새로 추가된 블록은 생성 직후 alias를 등록해야 다음 action에서 사용할 수 있다.

## alias 후보

하나의 블록은 여러 이름으로 들어올 수 있다.

| 원천 | 예 |
|---|---|
| AI 응답 | `id`, `alias` |
| 프론트 상태 | `nodeId`, `blockId` |
| 백엔드 응답 | `node_id`, `block_id` |
| 데이터 블록 특수 alias | `tdl`, `data`, `source` |

## 링크 정규화

링크도 필드명이 여러 형태로 들어올 수 있다.

```text
fromBlockId / from_block_id / fromAlias / source / originId
toBlockId   / to_block_id   / toAlias   / target / targetId
```

정규화 레이어는 이 값을 모두 읽어 내부적으로 같은 `from`, `to`, `fromSlot`, `toSlot` 형태로 만든다.

## 슬롯

분석 워크플로우에서는 하나의 블록이 여러 출력 슬롯을 가질 수 있다.

예:

- train 데이터 출력
- test 데이터 출력
- chart 결과 출력

그래서 링크는 단순한 edge가 아니라 `fromSlot`, `toSlot`까지 포함한 edge로 다뤄야 한다.

## 한 줄 정리

블록 Alias Map은 **AI가 말한 논리적 블록 이름과 UI가 가진 실제 blockId 사이를 잇는 번역표**이다.

## 관련

- [[Workflow Action]]
- [[Agent 응답 정규화]]
- [[LangGraph Edge]]
- [[캔버스 적용 오케스트레이터]]
