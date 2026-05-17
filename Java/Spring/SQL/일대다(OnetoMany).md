- 일대다(One-to-Many, 1:N) 관계는 **한 테이블의 하나의 레코드가 다른 테이블의 여러 레코드에 대응**되는 관계이다.
- 예: 한 명의 사용자(User)가 여러 게시글(Post)을 가진다 → User(1) : Post(N).
- [[외래 키(Foreign Key)]]는 항상 **N(다) 쪽**에 위치한다.

## 예시

- 학생(1) : 점수 여러 개(N)
- 사용자(1) : 주문 여러 개(N)
- 카테고리(1) : 게시글 여러 개(N)

## JPA 매핑

### 단방향 (Category → Post만 참조)

```java
@Entity
public class Category {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany
    @JoinColumn(name = "category_id")   // Post 테이블의 FK 컬럼
    private List<Post> posts = new ArrayList<>();
}
```

- 단방향 `@OneToMany`는 FK가 자식(Post) 테이블에 있지만 부모(Category)가 관리하므로 UPDATE 쿼리가 추가 발생한다.
- **실무에서는 단방향 @OneToMany보다 @ManyToOne 단방향을 권장한다.**

### 양방향 (Category ↔ Post 서로 참조)

```java
@Entity
public class Category {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany(mappedBy = "category")   // Post의 category 필드가 주인
    private List<Post> posts = new ArrayList<>();
}

@Entity
public class Post {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "category_id")   // 연관관계 주인 (FK 보유)
    private Category category;
}
```

## @OneToMany 주요 옵션

| 옵션 | 설명 |
| ---- | ---- |
| `mappedBy` | 연관관계 주인 필드명 지정 (주인이 아닌 쪽에 설정) |
| `fetch` | 기본값 `LAZY` (지연 로딩) |
| `cascade` | 영속성 전파 (`CascadeType.ALL` 등) |
| `orphanRemoval` | 컬렉션에서 제거 시 자동 DELETE |

## ERD 표현

```
Category (1) ←—————— Post (N)
   id PK            id PK
   name             title
                    category_id FK
```

## 관련

- [[@OneToMany]]
- [[@ManyToOne]]
- [[다대일(ManyToOne)]]
- [[단방향]]
- [[양방향]]
- [[연관 관계(Relationships)]]
- [[외래 키(Foreign Key)]]
- [[cascade]]
- [[orphanRemoval]]
