- Lombok은 **[[어노테이션(Annotation)]] 하나로 반복적인 Java 보일러플레이트 코드를 자동 생성**하는 라이브러리이다.
- `@Getter`, `@Setter`, `@RequiredArgsConstructor`, `@Builder` 등의 어노테이션을 붙이면 컴파일 시점에 코드가 자동 생성된다.
- Spring Boot 프로젝트에서 거의 필수적으로 사용한다.

## 주요 어노테이션

### @Getter / @Setter

```java
@Getter
@Setter
public class Post {
    private String title;
    private String content;
    // Lombok이 자동 생성:
    // public String getTitle() { return title; }
    // public void setTitle(String title) { this.title = title; }
}

// 클래스 레벨 또는 필드 레벨 개별 적용 가능
public class Post {
    @Getter private String title;        // title만 getter
    @Getter @Setter private String content;  // content는 getter + setter
}
```

- [[JPA(Java Persistence API)]] [[엔티티(entity)]]에서는 `@Setter`를 지양한다 — 무분별한 상태 변경 방지.
- [[영속성 컨텍스트(Persistence Context)]]의 변경 감지(Dirty Checking)와 함께 `setter` 대신 의미 있는 메서드를 사용한다.

---

### @RequiredArgsConstructor (가장 많이 사용)

```java
@Service
@RequiredArgsConstructor   // final 필드로 생성자 자동 생성
public class PostService {

    private final PostRepository postRepository;  // final → 생성자 주입
    private final CategoryUseCase categoryUseCase;

    // Lombok이 자동 생성:
    // public PostService(PostRepository postRepository, CategoryUseCase categoryUseCase) {
    //     this.postRepository = postRepository;
    //     this.categoryUseCase = categoryUseCase;
    // }
}
```

- [[DI(Dependency Injection)]] 생성자 주입의 표준 패턴 — `final` + `@RequiredArgsConstructor`.
- `@Autowired` 없이도 [[스프링 컨테이너(Spring Container)]]가 자동으로 주입한다.

---

### @Builder

```java
@Builder
@Getter
public class CreatePostCommand {
    private final String title;
    private final String content;
    private final String status;
}

// 빌더 패턴으로 객체 생성
CreatePostCommand command = CreatePostCommand.builder()
    .title("제목")
    .content("내용")
    .status("DRAFT")
    .build();
```

- [[객체(Object)]] 생성 시 어떤 필드에 어떤 값을 넣는지 명확하게 표현한다.
- 매개변수가 많을 때 가독성이 크게 향상된다.
- JPA 엔티티에서는 `@Builder` + `@NoArgsConstructor(access = AccessLevel.PROTECTED)` 조합 권장.

---

### @NoArgsConstructor / @AllArgsConstructor

```java
@Entity
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)  // JPA 기본 생성자 (직접 호출 차단)
@AllArgsConstructor   // 모든 필드 생성자
@Builder              // 빌더 패턴
public class Post {
    @Id @GeneratedValue
    private Long id;
    private String title;
    private String content;
}
```

| 어노테이션 | 설명 |
| ---- | ---- |
| `@NoArgsConstructor` | 파라미터 없는 기본 생성자 생성 |
| `@NoArgsConstructor(access = PROTECTED)` | JPA 엔티티 필수 — 외부에서 직접 생성 차단 |
| `@AllArgsConstructor` | 모든 필드를 받는 생성자 생성 |
| `@RequiredArgsConstructor` | `final` 또는 `@NonNull` 필드만 생성자 |

---

### @ToString

```java
@ToString(exclude = {"password", "posts"})  // 민감 정보 및 순환 참조 제외
@Getter
public class User {
    private Long id;
    private String email;
    private String password;      // 제외됨
    private List<Post> posts;     // 순환 참조 방지를 위해 제외
}
```

- **주의**: 연관관계가 있는 [[엔티티(entity)]]에서 `@ToString`을 무분별하게 사용하면 **순환 참조**로 StackOverflowError 발생.

---

### @EqualsAndHashCode

```java
@EqualsAndHashCode(of = "id")   // id 기준으로만 equals/hashCode
@Getter
public class Post {
    private Long id;
    private String title;
}
```

- JPA 엔티티에서는 `@EqualsAndHashCode` 주의 — `id`만 기준으로 사용하거나, 직접 오버라이드하는 것을 권장.

---

### @Data — 사용 주의

```java
@Data   // @Getter + @Setter + @RequiredArgsConstructor + @ToString + @EqualsAndHashCode
public class UserDto {
    private String name;
    private String email;
}
```

- DTO에서는 `@Data`를 쓸 수 있지만, **JPA 엔티티에서는 사용 금지** — `@Setter`가 포함되어 불변성을 해침.

---

### @Slf4j — 로깅

```java
@Slf4j   // log 변수 자동 생성
@Service
public class PostService {
    public Post findById(Long id) {
        log.info("게시글 조회: id={}", id);
        log.debug("상세 디버그 정보");
        log.warn("경고 상황");
        log.error("에러 발생: {}", id);
        return postRepository.findById(id).orElseThrow();
    }
}
```

## 어노테이션 조합 패턴

```java
// JPA 엔티티 표준 패턴
@Entity
@Table(name = "posts")
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Builder
public class Post { ... }

// 서비스 레이어 표준 패턴
@Service
@RequiredArgsConstructor
@Slf4j
public class PostService { ... }

// DTO 패턴
@Getter
@Builder
public class PostResponse { ... }
```

## 관련

- [[롬복(lombok)]]
- [[JPA(Java Persistence API)]]
- [[엔티티(entity)]]
- [[DI(Dependency Injection)]]
- [[영속성 컨텍스트(Persistence Context)]]
