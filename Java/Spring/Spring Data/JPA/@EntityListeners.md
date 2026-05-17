- `@EntityListeners`는 **[[엔티티(entity)]]의 생명주기 이벤트(저장, 수정, 삭제 등)에 콜백 리스너 클래스를 등록**하는 [[어노테이션(Annotation)]]이다.
- [[JPA(Java Persistence API)]] [[Auditing]] 기능을 활성화할 때 `AuditingEntityListener.class`를 인자로 전달하여 `createdAt`, `updatedAt` 등을 자동으로 관리한다.
- [[@MappedSuperClass]]와 함께 사용해 `BaseEntity`에 선언하면 모든 자식 [[엔티티(entity)]]에 자동 적용된다.

## 기본 사용 — JPA Auditing

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)   // JPA 제공 리스너 등록
public abstract class BaseEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    private LocalDateTime updatedAt;

    @CreatedBy
    @Column(updatable = false)
    private String createdBy;

    @LastModifiedBy
    private String updatedBy;
}
```

- `@CreatedDate` / `@LastModifiedDate` — 생성/수정 시각을 `LocalDateTime`으로 자동 저장.
- `@CreatedBy` / `@LastModifiedBy` — [[Spring Security]]의 `SecurityContextHolder`에서 현재 사용자를 자동으로 읽는다.
- `@Column(updatable = false)` — 최초 저장 이후 절대 변경되지 않도록 제한.

## @EnableJpaAuditing 필수 설정

```java
@SpringBootApplication
@EnableJpaAuditing   // Auditing 활성화 — 없으면 @CreatedDate 등이 동작하지 않음
public class BlogApplication {
    public static void main(String[] args) {
        SpringApplication.run(BlogApplication.class, args);
    }
}
```

- `@EnableJpaAuditing`이 없으면 리스너가 등록되어도 시간/사용자가 채워지지 않는다.

## @CreatedBy / @LastModifiedBy 설정 (AuditorAware)

```java
@Configuration
public class AuditingConfig implements AuditorAware<String> {

    @Override
    public Optional<String> getCurrentAuditor() {
        // Spring Security에서 현재 사용자 이름 추출
        return Optional.ofNullable(SecurityContextHolder.getContext())
            .map(SecurityContext::getAuthentication)
            .filter(Authentication::isAuthenticated)
            .map(Authentication::getName);
    }
}
```

```java
// @EnableJpaAuditing에 auditorAwareRef 추가
@EnableJpaAuditing(auditorAwareRef = "auditingConfig")
```

- `AuditorAware<T>` 빈을 등록해야 `@CreatedBy`, `@LastModifiedBy`가 동작한다.
- 익명 사용자 처리 시 `Optional.empty()` 반환 → 해당 필드를 null로 유지.

## 커스텀 리스너 등록

```java
// 커스텀 리스너 클래스
public class PostAuditListener {

    @PrePersist
    public void prePersist(Post post) {
        // 저장 직전 호출
    }

    @PostPersist
    public void postPersist(Post post) {
        // 저장 직후 호출
    }

    @PreUpdate
    public void preUpdate(Post post) {
        // 수정 직전 호출
    }

    @PostLoad
    public void postLoad(Post post) {
        // 조회 직후 호출
    }
}

// 엔티티에 커스텀 리스너 적용
@Entity
@EntityListeners({AuditingEntityListener.class, PostAuditListener.class})
public class Post extends BaseEntity { ... }
```

## 지원 콜백 어노테이션

| 어노테이션 | 시점 |
| ---- | ---- |
| `@PrePersist` | `persist()` 호출 직전 |
| `@PostPersist` | `persist()` 완료 직후 |
| `@PreUpdate` | `merge()` / 변경 감지 직전 |
| `@PostUpdate` | 업데이트 완료 직후 |
| `@PreRemove` | `remove()` 직전 |
| `@PostRemove` | 삭제 완료 직후 |
| `@PostLoad` | 조회 후 [[영속성 컨텍스트(Persistence Context)]]에 로딩 직후 |

## 관련

- [[@MappedSuperClass]]
- [[JPA(Java Persistence API)]]
- [[엔티티(entity)]]
- [[어노테이션(Annotation)]]
- [[영속성 컨텍스트(Persistence Context)]]
- [[Spring Security]]
