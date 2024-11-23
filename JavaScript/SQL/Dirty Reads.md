- 아직 commit 되지 않은 값을 다른 [[트랜잭션(Transaction)]]이 읽는 경우이다.


## 예시

- 변경한 데이터([[행(Row)]])를 커밋하지 않고 롤백할 경우 롤백 전에 데이터를 읽은 다른 [[트랜잭션(Transaction)]]은 잘못된 정보로 로직을 진행한다.
- Read Committed [[트랜잭션(Transaction)]]으로 해결 가능하다.


```sql
-- 기본값 1000

-- Transaction 1
BEGIN TRANSACTION;
UPDATE account SET balance = balance - 100 WHERE id = 1; -- balance = 900

-- Transaction 2 (어떠한 상황에 의해 동시 실행됨)
BEGIN TRANSACTION;
SELECT balance FROM account WHERE id = 1; -- 900 반환

-- Transaction 1 롤백
ROLLBACK; -- Balance 100으로되돌림

-- Transaction 2 진행
-- Transaction 2에서 읽은 balance 값은 잘못된 값임
```