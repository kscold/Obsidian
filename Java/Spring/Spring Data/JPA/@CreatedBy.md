- `@CreatedBy`는 Spring Data [[Auditing]] 기능에서 **엔티티가 생성될 때 생성자 정보를 자동으로 기록**하는 [[어노테이션(Annotation)]]이다.
- `@LastModifiedBy`와 함께 `AuditorAware<T>` 구현체를 등록해야 동작한다.
- 보통 [[Spring Security]]의 `SecurityContextHolder`에서 현재 로그인 사용자 ID를 가져와 기록한다.

## 설정 순서

### 1. BaseEntity에 @CreatedBy 선언

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdAt;

    @LastModifiedDate
    private LocalDateTime updatedAt;

    @CreatedBy
    @Column(updatable = false)
    private String createdBy;      // 생성자 ID/이름

    @LastModifiedBy
    private String lastModifiedBy; // 최종 수정자 ID/이름
}
```

### 2. AuditorAware 구현체 등록

```java
@Component
public class SecurityAuditorAware implements AuditorAware<String> {

    @Override
    public Optional<String> getCurrentAuditor() {
        Authentication auth = SecurityContextHolder.getContext().getAuthentication();
        if (auth == null || !auth.isAuthenticated()) {
            return Optional.of("system");
        }
        return Optional.of(auth.getName());   // 현재 로그인 사용자 이름(ID)
    }
}
```

### 3. Application 클래스에 Auditing 활성화

```java
@EnableJpaAuditing(auditorAwareRef = "securityAuditorAware")
@SpringBootApplication
public class BlogApplication { ... }
```

## @CreatedDate vs @CreatedBy

| 어노테이션 | 기록 내용 | 의존 |
| ---- | ---- | ---- |
| `@CreatedDate` | 생성 시각 (자동) | `@EnableJpaAuditing` |
| `@CreatedBy` | 생성자 식별값 | `@EnableJpaAuditing` + `AuditorAware` 구현 |
| `@LastModifiedDate` | 수정 시각 (자동) | `@EnableJpaAuditing` |
| `@LastModifiedBy` | 최종 수정자 | `@EnableJpaAuditing` + `AuditorAware` 구현 |

## 관련

- [[@CreatedDate]]
- [[@LastModifiedDate]]
- [[Auditing]]
- [[인증(Authentication)]]
- [[Spring Security]]
- [[JPA(Java Persistence API)]]
