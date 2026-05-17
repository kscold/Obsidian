- `GROUP BY`는 **동일한 값을 가진 행들을 하나의 그룹으로 묶어 집계 함수를 적용**하는 SQL 절이다.
- 주로 `COUNT`, `SUM`, `AVG`, `MAX`, `MIN` 같은 집계 함수와 함께 사용한다.
- SQL 실행 순서: `FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT`

## 기본 문법

```sql
SELECT 컬럼, 집계함수(컬럼)
FROM 테이블
WHERE 조건           -- 그룹화 전 필터
GROUP BY 컬럼
HAVING 집계조건      -- 그룹화 후 필터
ORDER BY 집계함수;
```

## 집계 함수

| 함수 | 설명 | 예시 |
| ---- | ---- | ---- |
| `COUNT(*)` | 행 수 | 카테고리별 게시글 수 |
| `COUNT(col)` | NULL 제외 행 수 | |
| `SUM(col)` | 합계 | 월별 매출 합계 |
| `AVG(col)` | 평균 | 평균 조회수 |
| `MAX(col)` | 최댓값 | 최고 점수 |
| `MIN(col)` | 최솟값 | 최저 가격 |
| `GROUP_CONCAT(col)` | 값 목록 연결 (MySQL) | 태그 목록 |

## 사용 예시

```sql
-- 카테고리별 게시글 수와 평균 조회수
SELECT category, COUNT(*) AS post_count, AVG(view_count) AS avg_views
FROM posts
WHERE status = 'published'
GROUP BY category
ORDER BY post_count DESC;
```

```sql
-- HAVING: 그룹화 후 조건 (게시글 3개 이상인 카테고리만)
SELECT category, COUNT(*) AS post_count
FROM posts
GROUP BY category
HAVING COUNT(*) >= 3
ORDER BY post_count DESC;
```

```sql
-- 복합 GROUP BY (연도+월별 통계)
SELECT YEAR(created_at) AS year, MONTH(created_at) AS month, COUNT(*) AS cnt
FROM orders
GROUP BY YEAR(created_at), MONTH(created_at)
ORDER BY year, month;
```

## WHERE vs HAVING

| 구분 | 적용 시점 | 집계함수 사용 |
| ---- | ---- | ---- |
| `WHERE` | GROUP BY 이전 (행 단위 필터) | 불가 |
| `HAVING` | GROUP BY 이후 (그룹 단위 필터) | 가능 |

```sql
-- 잘못된 예 (WHERE에 집계함수 사용 불가)
WHERE COUNT(*) > 3    -- 에러

-- 올바른 예
HAVING COUNT(*) > 3
```

## JPA에서 집계 쿼리

```java
// JPQL 집계
@Query("SELECT p.category, COUNT(p) FROM Post p GROUP BY p.category")
List<Object[]> countByCategory();

// Spring Data JPA Projection
@Query("SELECT p.category AS category, COUNT(p) AS count FROM Post p GROUP BY p.category")
List<CategoryCount> countByCategory();

public interface CategoryCount {
    String getCategory();
    Long getCount();
}
```

## 관련

- [[SELECT]]
- [[WHERE]]
- [[JOIN]]
- [[SQL]]
- [[집계(Aggregation)]]
