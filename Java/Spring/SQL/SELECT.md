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