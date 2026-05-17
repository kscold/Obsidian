- [[SQL]] 명령어로 특정 [[테이블(Table)]]의 데이터를 가져오고 싶을 때 사용하는 명령어이다.

- 전체 데이터를 가져오고 싶을 때 SELECT 명령문을 활용하여 * 전체 데이터를 가져온다.
- 즉, [[레코드(Record)]]를 가져온다.

## 문법

```sql
SELECT 
	속성명1, 속성명2, 속성명3
FROM
	테이블명
WHERE
	조건;
```

```sql
SELECT * FROM 테이블명
```

## 유저 생성 및 확인(MySQL)

- [[CREATE]]를 통해 user [[테이블(Table)]]에 아이디와 비빌번호 정보를 생성한 뒤, user 테이블을 선택해 확인한다.

```sql
create user 'kscold'@'localhost' identified by 'Tmdcks6502@'; -- user 아이디를 생성하는 코드  
select `user` from `mysql`.user; -- user가 제대로 생성됐는지 확인하는 코드
```

```sql
show grants for 'kscold'@'localhost'; -- 권한이 있는지 확인
-- GRANT USAGE ON *.* TO `kscold`@`localhost`로 뜬다면 아무런 권한이 없다는 말임

grant all on 'board'.* to 'kscold'@'localhost' with grant option; -- 모든 권한을 위임
-- 제대로 권한이 들어 갔다면 GRANT ALL PRIVILEGES ON `board`.* TO `kscold`@`localhost` WITH GRANT OPTION 모든 권한이 추가된 것을 확인할 수 있음


flush privileges; -- 정상적으로 동작하는지 log를 통해 확인할 수 있음
```
## 예시

- 밑에 코드는 orders [[테이블(Table)]]에서 특정 [[속성(Attribute)]] order_no, created_at, payment_method의 데이터를 불러오는 예시이다.

```sql
SELECT order_no, created_at, payment_method FROM orders
```

- 밑에 예시 코드는 mens [[테이블(Table)]]의 [[레코드(Record)]]를 보는 여러 방법이다.

```sql
-- 모두 보기
SELECT * FROM mens;

-- 칼럼(필드 혹은 속성) 선택해 보기
SELECT _id, name FROM mens;

-- 조건 선택해 보기
SELECT * FROM mens WHERE name = 'Kidongg' AND age = '27';
```