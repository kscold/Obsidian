- AssertJ는 **Java 테스트에서 체이닝(메서드 연결) 방식으로 직관적인 단언(Assertion)을 작성**하는 오픈소스 라이브러리이다.
- [[JUnit5]]와 함께 사용하며, Spring Boot Test에 기본으로 포함되어 있다.
- JUnit의 `assertEquals(expected, actual)` 보다 **가독성이 훨씬 높고** 실패 시 메시지도 상세하다.
- [[BDD]]의 [[Given-When-Then]] 패턴의 Then 단계에서 주로 사용한다.

## assertThat() — 값 검증

```java
import static org.assertj.core.api.Assertions.*;

// 기본 동등 비교
assertThat(post.getTitle()).isEqualTo("Spring Boot 입문");
assertThat(post.getId()).isNotNull();
assertThat(post.getStatus()).isEqualTo("PUBLISHED");

// 숫자 비교
assertThat(post.getViewCount()).isGreaterThan(0);
assertThat(post.getViewCount()).isBetween(1, 1000);
assertThat(score).isCloseTo(3.14, offset(0.01));

// Boolean
assertThat(post.isPublished()).isTrue();
assertThat(post.isDeleted()).isFalse();

// Null 체크
assertThat(result).isNull();
assertThat(result).isNotNull();

// 문자열
assertThat(title).contains("Spring");
assertThat(title).startsWith("Spring").endsWith("Boot");
assertThat(title).hasSize(14);
assertThat(slug).matches("[a-z0-9-]+");  // 정규식 패턴
```

## 컬렉션 검증

```java
List<Post> posts = postService.findAll();

// 크기
assertThat(posts).hasSize(3);
assertThat(posts).isNotEmpty();
assertThat(posts).isEmpty();

// 포함 여부
assertThat(posts).contains(post1, post2);
assertThat(posts).containsExactly(post1, post2, post3);     // 순서까지 일치
assertThat(posts).containsExactlyInAnyOrder(post3, post1, post2);  // 순서 무관

// 특정 필드 추출 후 검증
assertThat(posts)
    .extracting("title")
    .containsExactly("제목1", "제목2", "제목3");

// 여러 필드 동시 검증
assertThat(posts)
    .extracting("title", "status")
    .containsExactly(
        tuple("제목1", "PUBLISHED"),
        tuple("제목2", "DRAFT")
    );

// 조건 필터링
assertThat(posts)
    .filteredOn(p -> p.getStatus().equals("PUBLISHED"))
    .hasSize(2);
```

## 객체 필드 검증

```java
Post post = postService.findById(1L);

// usingRecursiveComparison: 재귀적으로 모든 필드 비교 (equals 구현 불필요)
assertThat(post)
    .usingRecursiveComparison()
    .ignoringFields("createdAt", "updatedAt")  // 제외할 필드 지정
    .isEqualTo(expectedPost);

// 특정 필드만 검증
assertThat(post)
    .hasFieldOrPropertyWithValue("title", "Spring Boot 입문")
    .hasFieldOrPropertyWithValue("status", "PUBLISHED");
```

## assertThatThrownBy — 예외 검증

```java
// 예외가 반드시 발생해야 통과
assertThatThrownBy(() -> postService.findById(999L))
    .isInstanceOf(ResourceNotFoundException.class)
    .hasMessage("Post not found: 999")
    .hasMessageContaining("not found");

// 예외 발생 + 원인(cause) 확인
assertThatThrownBy(() -> service.create(command))
    .isInstanceOf(DuplicateResourceException.class)
    .hasCauseInstanceOf(DataIntegrityViolationException.class);
```

## assertThatCode — 예외 발생/미발생 검증

```java
// 예외가 발생하지 않아야 통과
assertThatCode(() -> postService.create(validCommand))
    .doesNotThrowAnyException();

// 예외가 발생해야 하지만, assertThatThrownBy보다 유연한 검증
assertThatCode(() -> invalidOperation())
    .isInstanceOf(IllegalArgumentException.class);
```

## assertThat vs assertThatThrownBy vs assertThatCode 비교

| 메서드 | 목적 | 예외 발생 시 |
| ---- | ---- | ---- |
| `assertThat(value)` | 일반 값 검증 | 테스트 실패 |
| `assertThatThrownBy(lambda)` | 예외 발생을 검증 | 예외 없으면 실패 |
| `assertThatCode(lambda)` | 예외 없음 또는 예외 검증 | `doesNotThrowAnyException()` 사용 |

## 실제 테스트 코드 종합 예시 (BDD 패턴)

```java
@DisplayName("PostService 테스트")
class PostServiceTest {

    private PostService postService;
    private FakePostRepository fakePostRepository;

    @BeforeEach
    void setUp() {
        fakePostRepository = new FakePostRepository();
        postService = new PostService(fakePostRepository);
    }

    @Test
    @DisplayName("제목과 내용으로 게시글을 생성하면 저장된 게시글을 반환한다")
    void createPost_success() {
        // Given
        CreatePostCommand command = new CreatePostCommand("제목", "내용", "PUBLISHED");

        // When
        Post post = postService.create(command);

        // Then
        assertThat(post.getId()).isNotNull();
        assertThat(post.getTitle()).isEqualTo("제목");
        assertThat(post.getStatus()).isEqualTo("PUBLISHED");
    }

    @Test
    @DisplayName("존재하지 않는 게시글 조회 시 ResourceNotFoundException이 발생한다")
    void findById_notFound_throwsException() {
        // When & Then
        assertThatThrownBy(() -> postService.findById(999L))
            .isInstanceOf(ResourceNotFoundException.class)
            .hasMessageContaining("999");
    }
}
```

## SoftAssertions — 여러 검증 한꺼번에

```java
// 일반 assertThat: 첫 번째 실패 시 멈춤
// SoftAssertions: 모든 검증 실행 후 전체 실패 목록 출력

@Test
void checkMultipleFields() {
    SoftAssertions soft = new SoftAssertions();

    soft.assertThat(post.getTitle()).isEqualTo("제목");
    soft.assertThat(post.getStatus()).isEqualTo("PUBLISHED");
    soft.assertThat(post.getViewCount()).isGreaterThanOrEqualTo(0);

    soft.assertAll();  // 실패한 검증 전부 출력
}
```

## 관련

- [[JUnit5]]
- [[BDD]]
- [[Given-When-Then]]
- [[@Test]]
- [[@BeforeEach]]
- [[MockMvc]]
- [[TDD(Test Driven Development)]]
