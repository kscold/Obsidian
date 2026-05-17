- `@Service`는 **[[서비스(Service)]] 레이어 클래스임을 나타내는 [[어노테이션(Annotation)]]**으로, [[스프링 컨테이너(Spring Container)]]가 해당 클래스를 [[Bean]]으로 등록한다.
- `@Component`의 특화형이며, [[비즈니스 로직(Business Logic)]]을 담당하는 클래스에 붙인다.
- [[컨트롤러(Controller)]]에서 [[DI(Dependency Injection)]]로 주입받아 사용한다.

## 기본 사용

```java
@Service
@RequiredArgsConstructor   // final 필드 생성자 주입 (Lombok)
public class PostService {

    private final PostRepository postRepository;

    public Post findById(Long id) {
        return postRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("게시글을 찾을 수 없습니다: " + id));
    }

    public Post create(CreatePostCommand command) {
        Post post = Post.builder()
            .title(command.getTitle())
            .content(command.getContent())
            .build();
        return postRepository.save(post);
    }
}
```

## @Service vs @Component vs @Repository vs @Controller

| 어노테이션 | 역할 | 위치 |
| ---- | ---- | ---- |
| `@Component` | 범용 Bean 등록 | 어디서든 |
| `@Service` | 비즈니스 로직 | application/service |
| `@Repository` | 데이터 접근 | adapter/out/persistence |
| `@Controller` | 웹 요청 처리 | adapter/in/web |

- 네 가지 모두 `@Component`를 메타 어노테이션으로 사용 — 스프링이 동일하게 Bean 등록.
- 의미론적 구분이 목적: AOP 적용 범위, 예외 변환(`@Repository`의 `DataAccessException`) 등이 달라짐.

## UseCase 인터페이스 구현 (헥사고날 아키텍처)

```java
// application/port/in/PostUseCase.java
public interface PostUseCase {
    Post create(CreatePostCommand command);
    Post findById(Long id);
}

// application/service/PostApplicationService.java
@Service
@RequiredArgsConstructor
public class PostApplicationService implements PostUseCase {

    private final PostRepository postRepository;     // domain/port/out
    private final CategoryUseCase categoryUseCase;   // 타 도메인 UseCase (인터페이스)

    @Override
    public Post create(CreatePostCommand command) {
        // 비즈니스 로직 처리
        Category category = categoryUseCase.findById(command.getCategoryId());
        Post post = Post.builder()
            .title(command.getTitle())
            .category(category)
            .build();
        return postRepository.save(post);
    }
}
```

- [[컨트롤러(Controller)]]는 `PostUseCase` 인터페이스만 참조 — `PostApplicationService` 직접 주입 금지.

## @Transactional과 함께 사용

```java
@Service
@RequiredArgsConstructor
@Transactional(readOnly = true)   // 기본적으로 읽기 전용 트랜잭션
public class PostService {

    private final PostRepository postRepository;

    public Post findById(Long id) {
        return postRepository.findById(id).orElseThrow();
    }

    @Transactional   // 쓰기 작업은 별도로 선언 (readOnly = false)
    public Post create(CreatePostCommand command) {
        return postRepository.save(Post.from(command));
    }
}
```

- `readOnly = true`는 [[영속성 컨텍스트(Persistence Context)]]의 Dirty Checking을 비활성화해 성능을 높인다.
- 쓰기 메서드에만 `@Transactional`을 추가하는 패턴이 표준.

## 관련

- [[서비스(Service)]]
- [[어노테이션(Annotation)]]
- [[비즈니스 로직(Business Logic)]]
- [[DI(Dependency Injection)]]
- [[스프링 컨테이너(Spring Container)]]
- [[컨트롤러(Controller)]]
- [[Bean]]
- [[@Transactional]]
- [[@RequiredArgsConstructor]]
