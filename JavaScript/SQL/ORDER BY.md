- [[SELECT]]를 할 때 데이터([[행(Row)]])를 원하는 필드([[열(Column)]])나 조건으로 데이터를 정렬하는데 사용하는 [[SQL(Structured Query Language)]]이다.


## 문법

```sql
SELECT 
	필드명
FROM
	테이블명
ORDER BY 필드명 -- ASC 또는 DESC 명시하지 않으면 기본값은 ASC
```

- 위의 [[쿼리(Query)]]에서 ORDER BY의 필드명ASC 또는 DESC 명시하지 않으면 기본값은 ASC으로 동작한다.


## 예시

- 아래와 같이 필드([[열(Column)]])의 정렬 기준이 두개 일때에는 release_date를 기준으로 먼저 내림차순 한 뒤에, 그 데이터들 중에서 revenue로 한번 더 내림차순을 진행한다.

```sql
SELECT
  *
FROM
  movies
ORDER BY
  release_date DESC,
  revenue DESC;
```
