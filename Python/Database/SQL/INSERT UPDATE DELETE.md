---
title: INSERT UPDATE DELETE
created: 2026-08-28
tags:
  - database
  - sql
  - syntax
---

# INSERT UPDATE DELETE

데이터를 바꾸는 문장들(DML). 구조를 바꾸는 [[DDL과 제약조건|DDL]]과 구분된다.

## INSERT

```sql
INSERT INTO blocks (block_id, name, category)
VALUES ('TSM0004', '선형 회귀', 'model');

-- 여러 건 (한 번의 왕복 — 훨씬 빠르다)
INSERT INTO blocks (block_id, name, category) VALUES
    ('TSM0004', '선형 회귀', 'model'),
    ('TSM0005', '로지스틱 회귀', 'model');

-- 조회 결과를 그대로 삽입
INSERT INTO blocks_archive (block_id, name)
SELECT block_id, name FROM blocks WHERE deprecated = 1;
```

열 목록을 생략하면 테이블 열 순서에 의존한다. **항상 열을 명시한다** — 열이 추가되는 순간 깨진다.

## upsert — MySQL/MariaDB 방식

```sql
-- 있으면 갱신, 없으면 삽입
INSERT INTO blocks (block_id, name, updated_at)
VALUES ('TSM0004', '선형 회귀', NOW())
ON DUPLICATE KEY UPDATE
    name = VALUES(name),
    updated_at = VALUES(updated_at);
```

- **유니크 키 위반이 감지될 때** 갱신으로 전환된다. 즉 [[유니크 인덱스]]가 전제다.
- `created_at`은 `UPDATE` 절에 넣지 않는다. 최초 시각이 보존돼야 [[upsert|멱등]]하다. Mongo의 `$setOnInsert`와 같은 의도다 ([[MongoDB 갱신 연산자]]).

```sql
-- 충돌 시 아무것도 안 함
INSERT IGNORE INTO blocks (...) VALUES (...);

-- 충돌 시 지우고 다시 삽입 (기존 행의 다른 열이 사라진다 — 주의)
REPLACE INTO blocks (...) VALUES (...);
```

| 문법 | 동작 |
|---|---|
| `ON DUPLICATE KEY UPDATE` | 지정 열만 갱신 — **권장** |
| `INSERT IGNORE` | 충돌 무시. 다른 에러까지 삼키므로 주의 |
| `REPLACE INTO` | DELETE + INSERT. FK·트리거가 함께 도는 부작용 |

## UPDATE

```sql
UPDATE blocks
SET    name = '선형 회귀', updated_at = NOW()
WHERE  block_id = 'TSM0004';

-- 계산식
UPDATE stats SET calls = calls + 1 WHERE node = 'plan';

-- 조인 갱신 (MySQL/MariaDB 문법)
UPDATE blocks b
JOIN   option_keys o ON o.block_id = b.block_id
SET    b.has_options = 1
WHERE  o.required = 1;
```

> **`WHERE` 없는 `UPDATE`는 전체 행을 바꾼다.** 운영 사고 1순위.
> 습관: 같은 `WHERE`로 `SELECT COUNT(*)`를 먼저 실행해 영향 행 수를 확인한다.

```sql
SELECT COUNT(*) FROM blocks WHERE category = 'model';   -- 42
UPDATE blocks SET reviewed = 1 WHERE category = 'model'; -- Rows matched: 42
```

`Rows matched`와 `Rows changed`는 다르다. 값이 이미 같으면 matched만 늘고 changed는 0이다 ([[MongoDB CRUD 메서드|Mongo의 matched/modified]]와 같은 구분).

## DELETE

```sql
DELETE FROM blocks WHERE deprecated = 1 AND updated_at < '2026-01-01';
DELETE FROM blocks WHERE block_id IN (SELECT block_id FROM stale);
```

| 문장 | 성질 |
|---|---|
| `DELETE` | 행 단위, `WHERE` 가능, 롤백 가능, 느림 |
| `TRUNCATE TABLE t` | 전체 삭제, DDL 취급, 매우 빠름, **롤백 불가**, AUTO_INCREMENT 초기화 |
| `DROP TABLE t` | 테이블 자체를 없앰 |

## 안전 습관

```sql
START TRANSACTION;
DELETE FROM blocks WHERE ...;
SELECT ROW_COUNT();     -- 영향 받은 행 수 확인
-- 예상과 다르면
ROLLBACK;
-- 맞으면
COMMIT;
```

[[트랜잭션(ACID)]]으로 감싸면 **되돌릴 기회**가 생긴다. 대량 변경은 항상 이렇게 한다.

대량 삭제는 나눠서 한다. 한 번에 100만 행을 지우면 락과 복제 지연이 심해진다.

```sql
DELETE FROM logs WHERE created_at < '2026-01-01' LIMIT 10000;   -- 반복 실행
```

수명이 정해진 데이터라면 애초에 자동 만료를 쓰는 게 낫다 — Mongo의 [[MongoDB 인덱스 실전|TTL 인덱스]]가 그 역할이다.

## 한 줄 정리

`INSERT ... ON DUPLICATE KEY UPDATE`가 SQL의 upsert이고, **`WHERE` 없는 UPDATE/DELETE**가 최대 위험이며, 트랜잭션으로 감싸는 게 방어다.

## 관련

- [[SQL]]
- [[DDL과 제약조건]]
- [[트랜잭션(ACID)]]
- [[유니크 인덱스]]
- [[upsert]]
- [[MongoDB 갱신 연산자]]
- [[SQL ↔ MongoDB 문법 대조표]]
