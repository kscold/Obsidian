---
title: SELECT 문법
created: 2026-08-28
tags:
  - database
  - sql
  - syntax
---

# SELECT 문법

[[SQL]]의 조회문 전체 골격. 순서대로 쓰지만 [[SQL 실행 순서|실행 순서는 다르다]].

```sql
SELECT   [DISTINCT] 열목록
FROM     테이블 [별칭]
[JOIN    다른테이블 ON 조건]
WHERE    행조건
GROUP BY 묶는기준
HAVING   묶은뒤조건
ORDER BY 정렬기준 [ASC|DESC]
LIMIT    개수 [OFFSET 건너뛸수];
```

## 열 고르기

```sql
SELECT *                          FROM blocks;   -- 전부 (운영 코드에선 쓰지 않는다)
SELECT block_id, name             FROM blocks;
SELECT block_id AS id, name AS 블록명 FROM blocks; -- 별칭
SELECT DISTINCT category          FROM blocks;   -- 중복 제거
```

`SELECT *`를 피하는 이유: 열이 추가되면 결과 모양이 조용히 바뀌고, 필요 없는 데이터까지 읽어 **커버링 인덱스**를 못 쓴다.

## 계산과 함수

```sql
SELECT
    block_id,
    UPPER(name)                      AS upper_name,
    CONCAT(block_id, ' · ', name)    AS label,
    LENGTH(description)              AS desc_len,
    ROUND(score, 2)                  AS score2,
    COALESCE(alias, name)            AS display,   -- 앞이 NULL이면 뒤
    DATE_FORMAT(created_at, '%Y-%m') AS ym
FROM blocks;
```

| 종류 | 예 |
|---|---|
| 문자열 | `CONCAT` `SUBSTRING` `TRIM` `REPLACE` `LENGTH` |
| 숫자 | `ROUND` `FLOOR` `CEIL` `ABS` `MOD` |
| 날짜 | `NOW()` `DATE_FORMAT` `DATE_ADD` `DATEDIFF` |
| NULL 처리 | `COALESCE` `IFNULL` `NULLIF` → [[NULL과 3값 논리]] |

## CASE — SQL의 if

```sql
SELECT block_id,
       CASE
           WHEN score >= 0.8 THEN 'high'
           WHEN score >= 0.5 THEN 'mid'
           ELSE 'low'
       END AS score_band
FROM blocks;
```

`CASE`는 `SELECT`뿐 아니라 `WHERE`·`ORDER BY`·집계 안에서도 쓴다.

```sql
SELECT category,
       COUNT(*)                                     AS total,
       SUM(CASE WHEN deprecated = 1 THEN 1 ELSE 0 END) AS deprecated_cnt
FROM blocks
GROUP BY category;
```

이 패턴(**조건부 집계**)이 실무에서 가장 자주 쓰인다. 한 번의 스캔으로 여러 지표를 뽑는다.

## 정렬

```sql
ORDER BY created_at DESC              -- 내림차순
ORDER BY category ASC, name DESC      -- 여러 기준
ORDER BY 2                            -- 열 번호 (권장하지 않음 — 열이 바뀌면 깨진다)
ORDER BY CASE category WHEN 'model' THEN 1 ELSE 2 END, name   -- 커스텀 순서
```

`ORDER BY`에서는 `SELECT`의 별칭을 쓸 수 있다. `WHERE`에서는 못 쓴다 ([[SQL 실행 순서]]).

## 개수 제한

```sql
LIMIT 20;              -- 상위 20건
LIMIT 20 OFFSET 40;    -- 41~60번째 (= 3페이지)
```

`OFFSET`은 건너뛴 행을 **실제로 읽는다.** 깊은 페이지일수록 느려지는 것은 [[MongoDB 커서와 페이지네이션|Mongo의 skip]]과 똑같다. 대안도 같다 — 마지막 키 기준 조회.

```sql
SELECT * FROM logs WHERE id > 10000 ORDER BY id LIMIT 20;   -- keyset
```

## 집합 연산

```sql
SELECT block_id FROM blocks_a
UNION      -- 중복 제거 (정렬 비용 있음)
SELECT block_id FROM blocks_b;

SELECT ... UNION ALL SELECT ...;   -- 중복 유지, 더 빠름
```

중복이 없다는 걸 알면 항상 `UNION ALL`을 쓴다.

## 한 줄 정리

`SELECT ... FROM ... WHERE ... GROUP BY ... HAVING ... ORDER BY ... LIMIT`이 전부이고, **조건부 집계(`SUM(CASE WHEN ...)`)** 가 실무의 주력이다.

## 관련

- [[SQL]]
- [[SQL 실행 순서]]
- [[WHERE와 조건식]]
- [[GROUP BY와 집계 함수]]
- [[NULL과 3값 논리]]
- [[JOIN]]
- [[SQL ↔ MongoDB 문법 대조표]]
