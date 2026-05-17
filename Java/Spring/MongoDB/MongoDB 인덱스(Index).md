- MongoDB 인덱스는 **쿼리 성능을 높이기 위해 특정 필드에 B-Tree 구조로 색인**을 만드는 기능이다.
- 인덱스 없이 컬렉션 전체를 순회(컬렉션 스캔)하면 데이터가 많을수록 성능이 급격히 저하된다.
- Spring Data MongoDB에서는 [[@Document]]의 `@Indexed`, `@CompoundIndex` 어노테이션으로 선언한다.

## 인덱스 종류

| 종류 | 설명 | 선언 방법 |
| ---- | ---- | ---- |
| 단일 인덱스 (Single Field) | 하나의 필드 | `@Indexed` |
| 복합 인덱스 (Compound) | 두 개 이상 필드 | `@CompoundIndex` |
| 유니크 인덱스 (Unique) | 중복 불허 | `@Indexed(unique = true)` |
| TTL 인덱스 | 만료 시간 자동 삭제 | `@Indexed(expireAfterSeconds = N)` |
| 텍스트 인덱스 (Text) | 전문 검색 | `@TextIndexed` |
| 해시 인덱스 (Hashed) | 샤딩에 사용 | MongoDB CLI에서 설정 |

## Spring Data MongoDB 어노테이션 사용

```java
@Document(collection = "posts")
@CompoundIndex(def = "{'authorId': 1, 'createdAt': -1}", name = "author_date_idx")
public class Post {

    @Id
    private String id;

    @Indexed                        // 단일 인덱스
    private String slug;

    @Indexed(unique = true)         // 유니크 인덱스
    private String externalId;

    private String authorId;

    @Indexed(expireAfterSeconds = 3600)   // TTL 인덱스 (1시간 후 삭제)
    private Instant expiresAt;

    private Instant createdAt;
}
```

## 복합 인덱스 설계

- 자주 함께 사용하는 필드는 복합 인덱스로 묶는다.
- 정렬 방향 `1`(오름차순) / `-1`(내림차순)을 쿼리 패턴에 맞춰 지정한다.

```java
// 글쓴이 기준으로 최신순 조회 쿼리에 최적화된 복합 인덱스
@CompoundIndex(def = "{'authorId': 1, 'createdAt': -1}")
```

## 인덱스 적용 확인 (MongoDB Shell)

```javascript
// 인덱스 목록 조회
db.posts.getIndexes()

// 쿼리 실행 계획 확인
db.posts.find({ slug: "my-post" }).explain("executionStats")
// "IXSCAN" 이면 인덱스 사용, "COLLSCAN" 이면 전체 스캔 → 인덱스 필요
```

## 주의사항

- 인덱스는 읽기 성능은 높이지만 **쓰기 성능을 저하**시킨다 (인덱스 갱신 오버헤드).
- 불필요한 인덱스는 제거한다.
- `@Indexed`가 실제로 DB에 생성되려면 `spring.data.mongodb.auto-index-creation=true` 설정 또는 `@EnableMongoRepositories` 필요.

## 관련

- [[@Document]]
- [[Spring Data MongoDB]]
- [[MongoTemplate]]
- [[MongoDB]]
