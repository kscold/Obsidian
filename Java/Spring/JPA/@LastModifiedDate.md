- `@LastModifiedDate`는 Spring Data [[JPA(Java Persistence API)]] [[Auditing]] 기능에서 [[엔티티(Entity)]]가 **마지막으로 수정된 시각**을 자동으로 기록하는 [[어노테이션(Annotation)]]이다.
- [[@CreatedDate]]와 함께 `BaseEntity` 혹은 `BaseTime` 클래스에 두고 상속받아 공통으로 쓰는 것이 일반적이다.

- `@EnableJpaAuditing`이 Application 클래스에 없으면 값이 항상 `null`로 저장되니 반드시 활성화 해야 한다.
- `updatable = false`를 붙이지 않으면 수정될 때마다 덮어씌워지므로 `@LastModifiedDate`에는 붙이지 않는다(반대로 `@CreatedDate`에는 `updatable = false`를 붙인다).

## 사용 예시

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTime {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    private LocalDateTime updatedAt;
}
```

```java
@Entity
public class Post extends BaseTime {
    // Post 필드들...
}
```

```java
// Application 클래스에 반드시 추가
@EnableJpaAuditing
@SpringBootApplication
public class BlogApplication {
    public static void main(String[] args) {
        SpringApplication.run(BlogApplication.class, args);
    }
}
```

## @CreatedDate와 비교

| 항목 | `@CreatedDate` | `@LastModifiedDate` |
| ---- | ---- | ---- |
| 기록 시점 | 최초 저장 시 1회 | 저장/수정 시마다 갱신 |
| `updatable = false` | 필수 (수정 금지) | 불필요 (계속 갱신) |
| 용도 | `createdAt` 필드 | `updatedAt` 필드 |

## 관련

- [[@CreatedDate]]
- [[@CreatedBy]]
- [[Auditing]]
- [[JPA(Java Persistence API)]]
- [[어노테이션(Annotation)]]
