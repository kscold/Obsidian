- 첫 Read 이후에 다른 [[트랜잭션(Transaction)]]에서 데이터([[행(Row)]])를 추가한 경우이다.


## 예시

- [[트랜잭션(Transaction)]]이 여러 데이터([[행(Row)]])를 불러오는 필터링 [[쿼리(Query)]]를 진행 후 다른 [[트랜잭션(Transaction)]]에서 [[쿼리(Query)]]의 조건에 맞는 새로운 데이터([[행(Row)]])를 생성 했을 때 같은 [[쿼리(Query)]]가 다른 결과를 반환하는 걸 이야기한다.
- Serializable [[트랜잭션(Transaction)]]으로 해결 가능하다.

```sql
-- 기본값
-- id   balacne
-- 1    1000
-- 2    1500

-- Transaction 1
BEGIN TRANSACTION;
SELECT * FROM account WHERE balance > 1000; -- account 2 반환

-- Transaction 2
BEGIN TRANSACTION;
INSERT INTO account (id, balance) VALUES (3, 1200);
COMMIT;

-- Transaction 1
SELECT * FROM account WHERE balance > 1000; -- account 2 and account 3 반환 (phantom read)
COMMIT;
```