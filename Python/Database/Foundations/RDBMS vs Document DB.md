---
title: RDBMS vs Document DB
created: 2026-08-28
tags:
  - database
  - mongodb
  - sql
---

# RDBMS vs Document DB

| | RDBMS (MariaDB) | Document DB ([[MongoDB]]) |
|---|---|---|
| 저장 단위 | 행 | JSON 문서 |
| 스키마 | DB가 강제 | 강제 안 함 (코드가 계약 소유) |
| 관계 | [[JOIN]]으로 질의 시점에 합침 | 문서 안에 미리 넣음 (비정규화) |
| 변경 | `ALTER TABLE` 필요 | 그냥 필드 추가하면 됨 |
| 강점 | [[트랜잭션(ACID)]], 복잡한 조인 | 모양이 자주 바뀌는 데이터, 통째 읽기 |

## 어느 쪽을 고르나

- **관계가 본질이면** RDBMS. 사용자·주문·결제처럼 서로를 참조하는 데이터.
- **한 덩어리로 읽고 쓰면** Document DB. 대화 세션, 실행 trace, 계획 스냅샷처럼 "이 ID의 것 통째로"가 대부분인 데이터.

프로젝트가 둘 다 쓰는 이유가 이것이다. 제품 업무 데이터는 MariaDB, Agent 원장은 MongoDB.

## 스키마 없음의 대가

편한 만큼 **아무도 안 막아 준다.** 필드 이름 오타, 타입 변화, 죽은 필드가 조용히 쌓인다. 그래서 컬렉션 스키마를 코드에 선언하고 [[유니크 인덱스]]로 최소한의 유일성을 DB에 위임한다.

## 한 줄 정리

RDBMS는 **DB가 규칙을 지켜 주고**, Document DB는 **내가 지켜야 한다.**

## 관련

- [[MongoDB]]
- [[SQL]]
- [[JOIN]]
- [[유니크 인덱스]]
