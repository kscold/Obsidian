- 뷰(View)는 하나 이상의 [[테이블(Table)]]에서 도출된 가상 테이블로, 실제 데이터를 저장하지 않고 [[쿼리(Query)]]를 저장한다.
- 뷰를 조회하면 내부적으로 저장된 쿼리가 실행된다.
- 복잡한 쿼리를 단순화하거나, 특정 사용자에게 데이터 일부만 노출하는 보안 목적으로도 사용한다.

## 뷰 생성 및 조회

```sql
-- 뷰 생성
CREATE VIEW active_users AS
SELECT id, name, email
FROM users
WHERE status = 'active';

-- 뷰 조회 (테이블처럼 사용)
SELECT * FROM active_users WHERE name LIKE '김%';

-- 뷰 수정
CREATE OR REPLACE VIEW active_users AS
SELECT id, name, email, created_at
FROM users
WHERE status = 'active';

-- 뷰 삭제
DROP VIEW active_users;
```

## 뷰의 장단점

| 구분 | 내용 |
|------|------|
| 장점 | 복잡한 쿼리 재사용, 보안(민감 데이터 숨기기), 논리적 독립성 |
| 단점 | 실제 데이터 미저장(성능 개선 없음), 일부 뷰는 UPDATE 불가 |

## 업데이트 가능한 뷰

- 단순 SELECT로만 구성되어 있고 GROUP BY, DISTINCT, 집계 함수가 없으면 INSERT/UPDATE가 가능하다.

```sql
-- 업데이트 가능한 뷰
UPDATE active_users SET name = '김철수' WHERE id = 1;

-- 업데이트 불가능한 뷰 (집계 포함)
CREATE VIEW user_count_by_city AS
SELECT city, COUNT(*) as cnt FROM users GROUP BY city;
-- → INSERT/UPDATE/DELETE 불가
```
