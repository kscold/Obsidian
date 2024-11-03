- [[SQL(Structured Query Language)]]에는  [[SELECT]]을 사용할 때, CASE에 따라 조건을 나눠서 데이터([[행(Row)]])을 다룰 수 있다. 



## 문법

- 사용 방법은 CASE [[키워드(Keyword)]]를 통해 조건 정의를 명시하고, WHEN을 사용하여 필드([[열(Column)]])의 조건을 명시하고 THEN으로 넣을 데이터([[행(Row)]])를 명시한다. 

```sql
SELECT
	*,
    CASE
        WHEN(조건A) THEN A
        WHEN(조건B) THEN B
		ELSE C
	END AS 원하는 컬럼명
FROM 테이블명;
```

- 조건 A 를 만족할 땐 A라 출력하고, B를 만족하면 B 그 외에는 다 C라고 출력하는 [[SQL(Structured Query Language)]] [[쿼리(Query)]]이다.