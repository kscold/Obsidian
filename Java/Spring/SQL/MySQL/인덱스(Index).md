- 인덱스(Index)는 **테이블의 특정 컬럼에 대한 검색 속도를 높이기 위해 별도로 만드는 자료구조**이다.
- MySQL의 기본 스토리지 엔진 InnoDB는 **B-Tree 인덱스**를 사용한다.
- 인덱스가 없으면 `WHERE` 조건 검색 시 테이블 전체를 순회(Full Table Scan)하므로 데이터가 많을수록 느려진다.

## 인덱스 종류

| 종류 | 설명 |
| ---- | ---- |
| Primary Key | 클러스터형 인덱스, 기본 키에 자동 생성 |
| Unique Index | 중복 불허, `UNIQUE` 제약과 함께 생성 |
| Composite Index | 여러 컬럼을 묶은 복합 인덱스 |
| Full-Text Index | 텍스트 전문 검색 (`MATCH ... AGAINST`) |
| Spatial Index | 공간 데이터 검색 |

## JPA에서 인덱스 선언

```java
@Entity
@Table(name = "posts", indexes = {
    @Index(name = "idx_slug", columnList = "slug", unique = true),
    @Index(name = "idx_author_created", columnList = "author_id, created_at DESC")
})
public class Post {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String slug;
    private String authorId;
    private LocalDateTime createdAt;
}
```

## 복합 인덱스 설계 원칙

- **카디널리티(Cardinality)**가 높은 컬럼(값이 많이 다른 컬럼)을 앞에 배치한다.
- 쿼리에서 `WHERE`, `ORDER BY`, `GROUP BY`로 자주 쓰이는 컬럼 조합을 기준으로 만든다.
- 복합 인덱스는 왼쪽부터 순서대로 사용된다(최좌측 접두사 규칙).

```sql
-- (author_id, status, created_at) 복합 인덱스가 있을 때
SELECT * FROM posts WHERE author_id = 1 AND status = 'published'   -- 인덱스 사용
SELECT * FROM posts WHERE status = 'published'                      -- 인덱스 미사용
```

## EXPLAIN으로 인덱스 사용 확인

```sql
EXPLAIN SELECT * FROM posts WHERE slug = 'my-post';
```

| 컬럼 | 확인 항목 |
| ---- | ---- |
| `type` | `ref`, `range`, `index` → 좋음 / `ALL` → Full Scan → 인덱스 필요 |
| `key` | 사용된 인덱스 이름 |
| `rows` | 스캔 예상 행 수 (적을수록 좋음) |
| `Extra` | `Using filesort`, `Using temporary` → 성능 주의 |

## 인덱스 주의사항

- 인덱스는 **읽기 성능 향상, 쓰기 성능 저하** — INSERT/UPDATE/DELETE 시마다 인덱스도 갱신.
- 인덱스가 너무 많으면 오히려 느려질 수 있다. 자주 조회되는 컬럼에만 선별적으로 생성.
- `LIKE '%검색어'` 처럼 앞에 와일드카드가 오면 인덱스를 타지 못한다.
- 컬럼에 함수를 적용하면 인덱스 무효 — `WHERE YEAR(created_at) = 2024` 대신 `WHERE created_at >= '2024-01-01'`.

## N+1 문제와 인덱스

- JPA에서 연관관계 조회 시 발생하는 N+1은 인덱스보다 `fetch join` 또는 `@BatchSize`로 해결.
- [[fetch Join]], [[즉시 로딩(Eager Loading)]], [[지연 로딩(Lazy Loading)]] 참고.

## 관련

- [[MySQL(MariaDB)]]
- [[관계형 데이터베이스(Relational DataBase)]]
- [[JPA(Java Persistence API)]]
- [[트랜잭션(Transaction)]]
