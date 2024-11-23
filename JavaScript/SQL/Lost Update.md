- 두 [[트랜잭션(Transaction)]]이 같은 데이터([[행(Row)]])를 업데이트해서 하나의 업데이트가 손실되는 경우이다.


## 예시

- 두 개의 [[트랜잭션(Transaction)]]이 같은 데이터([[행(Row)]])을 일고 업데이트를 한다.
- 나중에 진행된 [[트랜잭션(Transaction)]]이 먼저 진행된 [[트랜잭션(Transaction)]]의 결과를 덮어쓴다.
- 먼저 진행된 [[트랜잭션(Transaction)]]의 작업은 유실된다.
- Optimistic Lock 전략으로 해결 가능하다.

```sql
-- Transaction 1
BEGIN TRANSACTION;
SELECT balance FROM account WHERE id = 1; -- 기본값 1000
UPDATE account SET balance = balance - 100 WHERE id = 1; -- 900으로 변경

-- Transaction 2 (어떠한 상황에 의해 동시 실행됨)
BEGIN TRANSACTION;
SELECT balance FROM account WHERE id = 1; -- 기본값 1000
UPDATE account SET balance = balance - 200 WHERE id = 1; -- 800으로 변경

-- Transaction 1 진행
COMMIT; -- Balance = 900

-- Transaction 2 진행
COMMIT; -- Balance = 800
```