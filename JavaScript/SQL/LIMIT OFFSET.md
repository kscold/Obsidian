- [[SQL(Structured Query Language)]]에서 [[SELECT]]를 수행 할 때, 조회할 데이터([[행(Row)]])의 크기와 기준점을  의미한다.


## 문법

- LIMIT은 조회([[SELECT]])할 결과의 개수를 의미한다.
- OFFSET이란 조회를 시작할 기준점을 의미한다.
- 이때 OFFSET이 정의되지 않았다면 데이터([[행(Row)]])는 0부터 시작한다.

```sql
SELECT -- 3
  *
FROM
  필드명 -- 1
  -- WHERE -- 2
  -- ORDER BY -- 4
LIMIT -- 6
  조회할 제한 수
OFFSET -- 5
  조회를 시작할 인덱스;
```

- 위의 주석 숫자는 [[WHERE]]과 [[ORDER BY]]가 같이 존재할 때, [[쿼리(Query)]]가 실행되는 순서이다.
- 따라서 면저 OFFSET을 확인하고 정의된 데이터의 갯수 만큼 건너뛰고 그 다음으로 LIMIT으로 정의된 수만큼 데이터([[행(Row)]])를 조회한다.


## 예시

- 예를 들어 아래와 같은 [[쿼리(Query)]]가 있을 경우 5000번째 행부터 10개의 데이터([[행(Row)]])을 읽겠다는 의미이다.

```sql
SELECT * FROM movies
LIMIT 10 OFFSET 5000;
```