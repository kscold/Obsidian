---
title: 트랜잭션(ACID)
created: 2026-08-28
tags:
  - database
  - sql
---

# 트랜잭션(ACID)

- 트랜잭션은 여러 변경을 **"전부 되거나 전부 안 되거나"** 하나로 묶는 단위다.
- 중간 상태가 남으면 안 되는 데이터(돈, 재고, 권한)가 여기 있어야 한다.

## ACID

| 글자 | 뜻 |
|---|---|
| **A**tomicity | 원자성. 일부만 반영되는 일이 없다 |
| **C**onsistency | 일관성. 제약조건을 깨는 상태로 끝나지 않는다 |
| **I**solation | 격리. 동시에 실행돼도 서로의 중간 상태를 보지 않는다 |
| **D**urability | 지속성. 커밋됐으면 죽어도 남는다 |

```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
UPDATE accounts SET balance = balance + 1000 WHERE id = 2;
COMMIT;   -- 둘 중 하나라도 실패하면 ROLLBACK
```

## 없는 곳에서는 어떻게 하나

[[MongoDB]]도 트랜잭션을 지원하지만 단일 문서 단위 원자성에 기대는 설계가 더 흔하다. AI Agent 원장은 트랜잭션 대신 **[[upsert]] 멱등성 + 원장 append**로 정합성을 만든다. 재실행이 안전하면 롤백이 덜 필요해진다.

## 한 줄 정리

트랜잭션은 **전부 아니면 전부 아님**을 보장하는 묶음이고, 없는 환경에서는 멱등성이 그 자리를 대신한다.

## 관련

- [[SQL]]
- [[upsert]]
- [[MongoDB]]
