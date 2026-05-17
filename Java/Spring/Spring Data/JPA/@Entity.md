- `@Entity`는 [[JPA(Java Persistence API)]]에서 제공하는 [[어노테이션(Annotation)]]으로, **이 어노테이션이 붙은 클래스가 DB 테이블과 매핑되는 엔티티**임을 나타낸다.
- `@Entity`가 붙은 클래스는 JPA가 관리하며, `ddl-auto` 설정에 따라 테이블을 자동 생성/검증한다.
- 기본 키 필드에 반드시 [[@Id]]를 선언해야 한다.

## 기본 사용

```java
@Entity
@Table(name = "posts")   // 테이블명 명시 (생략 시 클래스명 그대로 사용)
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Post {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 200)
    private String title;

    @Column(columnDefinition = "TEXT")
    private String content;

    @Enumerated(EnumType.STRING)
    private PostStatus status;

    private LocalDateTime createdAt;
}
```

## @Table 주요 속성

| 속성 | 설명 |
| ---- | ---- |
| `name` | 실제 테이블명 지정 (기본: 클래스명) |
| `indexes` | 인덱스 정의 (`@Index` 배열) |
| `uniqueConstraints` | 복합 유니크 제약 |
| `schema` | 스키마 지정 |

```java
@Entity
@Table(name = "posts", indexes = {
    @Index(name = "idx_post_slug", columnList = "slug", unique = true),
    @Index(name = "idx_post_author_created", columnList = "author_id, created_at DESC")
})
public class Post { ... }
```

## @Column 주요 속성

| 속성 | 기본값 | 설명 |
| ---- | ---- | ---- |
| `name` | 필드명 | 실제 컬럼명 |
| `nullable` | `true` | NOT NULL 제약 |
| `unique` | `false` | UNIQUE 제약 |
| `length` | 255 | VARCHAR 길이 |
| `columnDefinition` | - | DDL 직접 지정 (`TEXT`, `JSONB` 등) |
| `updatable` | `true` | UPDATE 시 포함 여부 |

## 주의사항

- `@Entity` 클래스는 **기본 생성자**(no-arg constructor)가 반드시 있어야 한다.
  - Lombok: `@NoArgsConstructor(access = AccessLevel.PROTECTED)` 권장 (직접 생성 방지)
- `final` 클래스나 `final` 필드에는 사용할 수 없다.
- 상속 구조가 필요한 경우 `@MappedSuperclass` 또는 `@Inheritance` 사용.

## 동일한 개념의 MongoDB 버전

- MongoDB에서는 [[@Document]] 어노테이션이 `@Entity`에 해당한다.
- `@Entity` → 관계형 DB 테이블 / `@Document` → MongoDB 컬렉션

## 관련

- [[JPA(Java Persistence API)]]
- [[@Id]]
- [[@MappedSuperclass]]
- [[Hibernate]]
- [[@Document]]
- [[엔티티(entity)]]
