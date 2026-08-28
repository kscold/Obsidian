---
title: SQL
created: 2026-08-28
tags:
  - database
  - sql
---

# SQL

- SQL은 **표(table)** 를 다루는 [[쿼리(Query)|쿼리]] 언어다.
- 행(row)과 열(column)이 있고, 열의 타입과 개수가 **미리 고정**된다.
- 프로젝트에서는 BigZAMi 제품 본체가 MariaDB를 쓰고, Spring이 JPA/MyBatis로 접근한다.

## 절 하나씩

| 절 | 뜻 | 예 |
|---|---|---|
| `SELECT` | 어떤 열을 볼지 | `SELECT id, name` |
| `FROM` | 어느 표에서 | `FROM users` |
| `WHERE` | 행 필터 | `WHERE status = 'ACTIVE' AND age >= 20` |
| `JOIN` | 다른 표를 키로 이어 붙임 | [[JOIN]] |
| `GROUP BY` | 묶어서 집계 | `GROUP BY category` + `COUNT(*)` |
| `HAVING` | 집계 **후** 필터 | `HAVING COUNT(*) > 3` |
| `ORDER BY` | 정렬 | `ORDER BY created_at DESC` |
| `LIMIT` | 개수 제한 | `LIMIT 20` |

## 관계형이 유일하게 잘하는 두 가지

- **[[JOIN]]** — 중복 없이 여러 표로 쪼개 두고(정규화) 질의 시점에 이어 붙인다.
- **[[트랜잭션(ACID)]]** — 여러 변경을 "전부 되거나 전부 안 되거나"로 묶는다.

이 둘이 필요 없으면 굳이 관계형일 이유가 약해진다.

## 실무 경계

AI Agent 파이썬 코드는 **MariaDB에 직접 붙지 않는다.** 필요한 값은 [[gRPC]]로 Spring이 넘겨준다. 경계를 넘으면 두 시스템이 같은 표를 서로 다른 규칙으로 쓰게 된다.

## 한 줄 정리

SQL은 **타입이 고정된 표에 조건·조인·집계를 선언하는 언어**다.

## 문법 노트

| 주제 | 노트 |
|---|---|
| 조회문 전체 골격 | [[SELECT 문법]] |
| 조건식 · LIKE · 인덱스 | [[WHERE와 조건식]] |
| NULL의 3값 논리 | [[NULL과 3값 논리]] |
| 집계와 HAVING | [[GROUP BY와 집계 함수]] |
| 쿼리 안의 쿼리 | [[서브쿼리]] |
| WITH · 순위 · 누적 | [[CTE와 윈도우 함수]] |
| 삽입·갱신·삭제·upsert | [[INSERT UPDATE DELETE]] |
| 테이블·제약조건·인덱스 DDL | [[DDL과 제약조건]] |
| 실행 계획 진단 | [[MySQL EXPLAIN 읽기]] |
| Mongo와 나란히 보기 | [[SQL ↔ MongoDB 문법 대조표]] |

## 관련

- [[SQL 실행 순서]]
- [[JOIN]]
- [[트랜잭션(ACID)]]
- [[RDBMS vs Document DB]]
- [[인덱스(Index)]]
