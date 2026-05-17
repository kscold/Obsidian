- fetch Join은 **[[JPQL(Java Persitence Query Language)]]에서 연관된 [[엔티티(entity)]]나 컬렉션을 한 번의 JOIN 쿼리로 함께 조회**하는 성능 최적화 기능이다.
- [[SQL]]의 JOIN과 달리, [[JPA(Java Persistence API)]]의 [[지연 로딩(Lazy Loading)]] 설정을 무시하고 즉시 연관 객체를 로딩한다.
- `join fetch` 명령어로 사용하며, [[N+1 문제(N+1 Problem)]]의 가장 기본적인 해결책이다.

## 기본 사용법

```java
// 일반 JPQL — N+1 문제 발생
@Query("SELECT p FROM Post p")
List<Post> findAll();

// fetch join JPQL — author를 함께 한 번에 조회
@Query("SELECT p FROM Post p JOIN FETCH p.author")
List<Post> findAllWithAuthor();

// 실제 실행 SQL
// SELECT p.*, u.*
// FROM posts p
// INNER JOIN users u ON p.author_id = u.id
```

## 단일 연관 (ManyToOne, OneToOne)

```java
// Post → User 다대일 관계
@Entity
public class Post {
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "author_id")
    private User author;
}

// fetch join으로 author를 한 번에 조회
@Query("SELECT p FROM Post p JOIN FETCH p.author WHERE p.status = :status")
List<Post> findByStatusWithAuthor(@Param("status") String status);
```

## 컬렉션 fetch join (OneToMany)

```java
// Post → Comments 일대다 관계
@Entity
public class Post {
    @OneToMany(mappedBy = "post", fetch = FetchType.LAZY)
    private List<Comment> comments;
}

// 컬렉션 fetch join
@Query("SELECT DISTINCT p FROM Post p JOIN FETCH p.comments WHERE p.id = :id")
Optional<Post> findByIdWithComments(@Param("id") Long id);
// DISTINCT 필수 — OneToMany는 조인 시 Post가 N배로 중복 반환됨
```

## 페이징과 컬렉션 fetch join 주의사항

```java
// 위험: 컬렉션 fetch join + 페이징 → HHH90003004 경고
// → JPA가 모든 데이터를 메모리에 올려놓고 페이징 처리
@Query("SELECT p FROM Post p JOIN FETCH p.comments")
Page<Post> findAll(Pageable pageable);  // 위험

// 안전한 방법 1: @BatchSize로 분리
@Query("SELECT p FROM Post p")
Page<Post> findAll(Pageable pageable);  // Post만 페이징

@OneToMany(mappedBy = "post")
@BatchSize(size = 100)
private List<Comment> comments;  // 별도 IN 쿼리로 조회

// 안전한 방법 2: DTO 직접 조회
@Query("SELECT new dto.PostSummary(p.id, p.title, COUNT(c)) " +
       "FROM Post p LEFT JOIN p.comments c GROUP BY p.id, p.title")
Page<PostSummary> findSummaries(Pageable pageable);
```

## 여러 연관 동시 fetch join 주의

```java
// 단일 연관(ManyToOne)은 여러 개 동시 fetch join 가능
@Query("SELECT p FROM Post p JOIN FETCH p.author JOIN FETCH p.category")
List<Post> findAllWithAuthorAndCategory();

// 컬렉션(OneToMany)은 2개 이상 동시 fetch join 불가 — MultipleBagFetchException
// 아래 코드는 예외 발생
@Query("SELECT p FROM Post p JOIN FETCH p.comments JOIN FETCH p.tags")
List<Post> findAll();  // MultipleBagFetchException!

// 해결: @BatchSize로 첫 번째 컬렉션만 fetch join, 나머지는 배치
```

## fetch join vs @EntityGraph 비교

| 항목 | fetch join | @EntityGraph |
| ---- | ---- | ---- |
| 방법 | JPQL 직접 작성 | 어노테이션 |
| 유연성 | 높음 (WHERE, ORDER BY 등) | 낮음 |
| 코드 량 | 많음 | 적음 |
| 동적 쿼리 | 가능 | 어려움 |
| 복잡한 조건 | 적합 | 단순 조건에 적합 |

```java
// @EntityGraph로 동일한 효과 (간단한 경우)
@EntityGraph(attributePaths = {"author"})
List<Post> findByStatus(String status);
```

## 관련

- [[JPA(Java Persistence API)]]
- [[JPQL(Java Persitence Query Language)]]
- [[N+1 문제(N+1 Problem)]]
- [[지연 로딩(Lazy Loading)]]
- [[즉시 로딩(Eager Loading)]]
- [[영속성 컨텍스트(Persistence Context)]]
- [[@EntityGraph]]
