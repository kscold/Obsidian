- 한 [[트랜잭션(Transaction)]]에서 데이터([[행(Row)]])를 두 번 읽을 때 다른 결과가 나오는 경우이다.


## 예시

- [[트랜잭션(Transaction)]]이 데이터([[행(Row)]])를 읽은 상태에서 다른 [[트랜잭션(Transaction)]]이 데이터([[행(Row)]])를 변경할 경우 같은 데이터([[행(Row)]])를 다시 읽었을 때 기존 읽었던 데이터([[행(Row)]])가 재구현되지 않은 현상을 이야기한다.
- Repeatable Read [[트랜잭션(Transaction)]]으로 해결 가능하다.


```sql
-- Transaction 1
BEGIN TRANSACTION;
SELECT balance FROM account WHERE id = 1; -- 기본값 1000

-- Transaction 2 (어떠한 상황에 의해 동시 실행됨)
BEGIN TRANSACTION;
UPDATE account SET balance = balance - 100 WHERE id = 1; -- 900으로 
COMMIT;

-- Transaction 1 continues
SELECT balance FROM account WHERE id = 1; -- 900 반환(non-repeatable read)
COMMIT;
```