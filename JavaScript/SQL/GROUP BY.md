- [[SQL(Structured Query Language)]]의 [[쿼리(Query)]]문에서 GROUP BY 문법은 동일한 범주의 데이터([[행(Row)]])를 묶어주는 역할을 한다.
- 동일한 범주를 갖는 데이터([[행(Row)]])를 하나로 묶어서 범주별 통계를 내주는 것을 의미한다.

- GROUP BY 문법은 주로 [[집계 함수(Aggregate Function)]]와 같이 사용되곤 한다.
- 여기서 집계 함수는 여러 행의 값을 더하거나, 평균값을 내거나, 개수를 세는 등 여러 개의 데이터에 관한 계산을 한다.가장 대표적인 집계 함수는 다음과 같다.

## 문법


```sql
SELECT -- 4
  director,
  SUM(revenue) AS total_revenue
FROM -- 1
  movies
WHERE -- 2
  director IS NOT NULL
  AND revenue IS NOT NULL
GROUP BY -- 3
  director -- 감독을 기준으로 행들을 병합
ORDER BY -- 5
  total_revenue DESC; -- 감독별 총 수익을 내림차순으로 정렬
```