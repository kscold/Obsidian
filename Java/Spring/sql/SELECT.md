- 특정 테이블의 데이터를 가져오고 싶을 때 사용하는 명령어이다.

전체 데이터를 가져오고 싶을 때 SELECT 명령문을 활용하여 * 전체 데이터를 가져온다.

밑에 코드는 임의의 orders 테이블에서 전체 필드 데이터를 불러오는 예시

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


밑에 코드는 orders 테이블에서 특정 필드 order_no, created_at, payment_method의 데이터를 불러오는 예시

```sql
SELECT order_no, created_at, payment_method FROM orders
```