---
title: LangGraph 운영용 메모리 저장소
created: 2026-06-23
tags:
  - langgraph
  - memory
  - operations
  - database
---

# LangGraph 운영용 메모리 저장소

- 실습에서는 [[LangGraph InMemorySaver]], [[LangGraph SqliteSaver]], [[LangGraph InMemoryStore]]로 메모리 상태 관리를 배운다.
- 하지만 실제 서비스에서는 더 안정적인 저장소가 필요하다.
- 사용자가 많아지고, 서버가 여러 대가 되고, 장애 복구가 중요해지면 RAM이나 로컬 파일만으로는 부족하다.

## 실습용과 운영용의 차이

| 구분 | 실습용 | 운영용 |
|---|---|---|
| 목적 | 개념 이해, 로컬 실행 | 안정적인 서비스 운영 |
| 저장 위치 | RAM, 로컬 SQLite 파일 | 서버형 DB, 캐시, 벡터 DB |
| 장애 복구 | 약함 | 백업/복구 필요 |
| 동시성 | 낮음 | 여러 사용자/서버 동시 접근 |
| 예시 | InMemorySaver, SqliteSaver, InMemoryStore | PostgreSQL, MySQL, Redis, Vector DB |

## Checkpointer 운영 저장소

Checkpointer는 그래프 실행 상태를 저장한다.

- 같은 `thread_id`의 대화를 이어가기 위해 필요하다.
- `interrupt` 후 다시 실행하기 위해 필요하다.
- 장애 후 복구하려면 영속 저장소가 필요하다.

운영에서는 보통 다음을 고려한다.

- PostgreSQL 기반 checkpointer
- 서버형 DB에 checkpoint 저장
- checkpoint 보존 기간 정책
- 사용자별/세션별 삭제 정책
- 민감정보 저장 여부 점검

## Store 운영 저장소

Store는 재사용할 지식을 저장한다.

- 사용자 선호
- 장기 프로필
- 프로젝트 지식
- 에이전트가 학습한 규칙
- 검색 가능한 장기 기억

운영에서는 보통 다음을 고려한다.

- PostgreSQL 또는 MySQL 같은 관계형 DB
- Redis 같은 빠른 key-value 저장소
- 벡터 DB를 이용한 의미 검색
- namespace 기반 사용자/프로젝트 분리
- 개인정보 삭제 요청 처리

## PostgreSQL과 MySQL 감각

- PostgreSQL은 LangGraph 생태계에서 운영용 checkpointer로 자주 언급되는 선택지다.
- MySQL도 애플리케이션의 일반 저장소로는 충분히 사용할 수 있다.
- 다만 LangGraph checkpointer/store로 직접 연결하려면 해당 DB에 맞는 구현체나 커스텀 저장 계층이 필요하다.
- 중요한 것은 DB 이름보다 저장 대상의 성격을 분리하는 것이다.

## 저장 대상별 선택

| 저장할 것 | 추천 저장 계층 |
|---|---|
| 같은 대화의 실행 상태 | Checkpointer |
| interrupt 후 재개 상태 | Checkpointer |
| 사용자 선호 | Store |
| 프로젝트 규칙 | Store |
| 문서 검색용 지식 | Vector DB / RAG 저장소 |
| 로그와 추적 | Observability 저장소 |

## 설계 기준

- 대화 상태와 장기 지식을 같은 테이블에 섞지 않는다.
- `thread_id`와 `namespace`를 명확히 분리한다.
- 민감정보가 저장되는지 먼저 확인한다.
- 저장 기간과 삭제 정책을 정한다.
- 운영에서는 tracing과 함께 저장 실패도 관측한다.

## 한 줄 요약

- 실습: InMemorySaver, SqliteSaver, InMemoryStore로 개념을 익힌다.
- 운영: PostgreSQL/MySQL/Redis/Vector DB 같은 외부 저장소를 사용한다.
- 핵심: Checkpointer는 실행 상태, Store는 재사용 지식이다.

## 관련

- [[LangGraph 메모리 상태 관리]]
- [[LangGraph Checkpointer]]
- [[LangGraph Store]]
- [[LangGraph SqliteSaver]]
- [[LangGraph InMemoryStore]]
- [[Observability]]
