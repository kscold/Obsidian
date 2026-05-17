- 인덱스(Index)는 [[테이블(Table)]]의 특정 [[열(Column)]]에 대한 검색 속도를 높이기 위한 별도의 자료 구조다.
- 책의 색인처럼 데이터베이스 엔진이 전체 테이블을 스캔하지 않고 원하는 행을 빠르게 찾을 수 있게 해준다.
- 조회 속도는 빠르지만, INSERT/UPDATE/DELETE 시 인덱스도 갱신해야 하므로 쓰기 성능은 저하된다.
- [[기본 키(Primary Key)]]에는 자동으로 인덱스가 생성된다.

## 인덱스 종류

| 종류 | 설명 |
|------|------|
| 클러스터드 인덱스(Clustered) | 테이블 데이터를 인덱스 기준으로 물리적으로 정렬. 테이블당 1개만 가능 (PRIMARY KEY) |
| 논클러스터드 인덱스(Non-clustered) | 별도 자료 구조로 포인터만 저장. 여러 개 생성 가능 |
| 복합 인덱스(Composite) | 여러 열을 합쳐 하나의 인덱스 생성. 선두 열 기준으로 탐색 |
| 유니크 인덱스(Unique) | 중복 값을 허용하지 않는 인덱스 |

## 인덱스 생성 및 삭제

```sql
-- 기본 인덱스 생성
CREATE INDEX idx_user_email ON users (email);

-- 유니크 인덱스
CREATE UNIQUE INDEX idx_user_email ON users (email);

-- 복합 인덱스 (성 + 이름 순으로 검색이 많을 때)
CREATE INDEX idx_name ON users (last_name, first_name);

-- 인덱스 삭제
DROP INDEX idx_user_email ON users;

-- 테이블 인덱스 목록 확인
SHOW INDEX FROM users;
```

## 인덱스 성능

```sql
-- EXPLAIN으로 쿼리 실행 계획 확인
EXPLAIN SELECT * FROM users WHERE email = 'test@example.com';
-- type: ref → 인덱스 사용 중
-- type: ALL → 풀 테이블 스캔 (인덱스 없음)
```

- 인덱스를 사용하지 못하는 경우:
  - `WHERE` 절에서 컬럼에 함수를 적용할 때 (`WHERE UPPER(email) = 'TEST@EXAMPLE.COM'`)
  - LIKE 앞부분에 와일드카드를 쓸 때 (`WHERE name LIKE '%홍'`)
  - 복합 인덱스의 선두 열이 아닌 열로만 조회할 때

## 언제 인덱스를 생성하나

- 자주 `WHERE`, `JOIN`, `ORDER BY`에 사용되는 열
- [[외래 키(Foreign Key)]] 열
- 중복이 적고 카디널리티(cardinality)가 높은 열
