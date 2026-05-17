- 쿼리(Query)는 [[데이터베이스(DataBase)]]에 특정 데이터를 요청하거나 조작하는 명령문이다.
- [[SQL(Structured Query Language)]]을 사용하여 작성하며, 데이터 조회(SELECT), 삽입(INSERT), 수정(UPDATE), 삭제(DELETE) 등의 작업을 수행한다.
- 쿼리는 사용자가 DBMS에 원하는 작업을 전달하는 유일한 수단이다.
- 쿼리의 성능은 인덱스, 조인 방식, 실행 계획(Execution Plan)에 따라 크게 달라진다.

## 쿼리의 종류 (DML / DDL / DCL)

| 분류 | 명령어 | 설명 |
|------|--------|------|
| DML (데이터 조작) | SELECT, INSERT, UPDATE, DELETE | 데이터 조회/수정 |
| DDL (데이터 정의) | CREATE, ALTER, DROP | 스키마 구조 변경 |
| DCL (데이터 제어) | GRANT, REVOKE | 접근 권한 관리 |
| TCL (트랜잭션 제어) | COMMIT, ROLLBACK | 트랜잭션 관리 |

## 기본 SELECT 쿼리

```sql
-- 테이블에서 원하는 열만 조회
SELECT id, name, email
FROM users
WHERE role = 'admin'
ORDER BY created_at DESC
LIMIT 10;
```

## 조건 쿼리 (WHERE)

```sql
-- 복합 조건으로 필터링
SELECT *
FROM orders
WHERE status = 'pending'
  AND created_at >= '2024-01-01'
  AND amount > 10000;
```

## 집계 쿼리 (GROUP BY + 집계 함수)

```sql
-- 카테고리별 평균 가격 조회
SELECT category, COUNT(*) AS total, AVG(price) AS avg_price
FROM products
GROUP BY category
HAVING AVG(price) > 5000
ORDER BY avg_price DESC;
```

## JOIN 쿼리

```sql
-- users 테이블과 orders 테이블을 user_id로 연결
SELECT u.name, o.id AS order_id, o.amount
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE o.status = 'completed';
```

## 쿼리 최적화 고려사항

- `SELECT *` 대신 필요한 컬럼만 명시하면 I/O를 줄일 수 있다.
- WHERE 조건에 인덱스가 걸린 컬럼을 사용하면 풀 스캔을 방지한다.
- `EXPLAIN` 또는 `EXPLAIN ANALYZE` 명령으로 실행 계획을 확인할 수 있다.

```sql
-- 실행 계획 확인
EXPLAIN SELECT * FROM users WHERE email = 'test@example.com';
```
