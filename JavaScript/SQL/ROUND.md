- [[SQL(Structured Query Language)]]에서 ROUND을 사용하여, [[쿼리(Query)]]의 결과를 소수점의 몇번째까지 표시할지 확인할 수 있다.


## 문법

```sql
SELECT ROUND(집계 함수(필드명), 2) -- SUM이나 AVG 등
FROM 테이블명;
```


## 예시

- 아래와 같이 movies의 revenue의 필드([[열(Column)]])의 데이터([[행(Row)]])들이 너무 커서 소수점이라고 가정하면, ROUND [[함수(Function)]]를 사용하여 [[쿼리(Query)]]의 결과값의 소수점을 제한할 수 있다.

```sql
SELECT ROUND(revenue, 2) FROM movies;
```