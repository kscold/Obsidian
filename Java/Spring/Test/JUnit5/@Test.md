- `@Test`는 **[[JUnit5]]에서 해당 [[메서드(Method)]]가 테스트 메서드임을 나타내는 [[어노테이션(Annotation)]]**이다.
- `@Test`가 붙은 메서드는 [[JUnit5]] 런타임이 자동으로 실행하며 독립적으로 수행된다.
- [[TDD(Test Driven Development)]] 및 [[BDD]] 기반 테스트의 기본 단위이다.

## 기본 사용

```java
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.*;

class CalculatorTest {

    @Test
    void 두_수를_더하면_합을_반환한다() {
        // Given
        int a = 3, b = 4;

        // When
        int result = Calculator.add(a, b);

        // Then
        assertThat(result).isEqualTo(7);
    }
}
```

## 예외 검증

```java
@Test
void 0으로_나누면_ArithmeticException이_발생한다() {
    // When & Then
    assertThatThrownBy(() -> Calculator.divide(10, 0))
        .isInstanceOf(ArithmeticException.class)
        .hasMessage("/ by zero");
}

// 예외 타입만 빠르게 검증할 경우
@Test
void 존재하지_않는_게시글_조회시_예외가_발생한다() {
    assertThatExceptionOfType(ResourceNotFoundException.class)
        .isThrownBy(() -> postService.findById(999L));
}
```

## 타임아웃 설정

```java
@Test
@Timeout(value = 500, unit = TimeUnit.MILLISECONDS)   // 500ms 초과 시 실패
void 조회는_500ms_안에_완료되어야_한다() {
    postService.findAll();
}
```

## 테스트 비활성화

```java
@Test
@Disabled("아직 구현 전 — 스펙 확정 후 활성화")
void 미구현_기능_테스트() {
    // 실행되지 않음
}
```

## JUnit 4 vs JUnit 5 비교

| 항목 | JUnit 4 | JUnit 5 |
| ---- | ---- | ---- |
| 어노테이션 | `@Test` (org.junit) | `@Test` (org.junit.jupiter.api) |
| 초기화 | `@Before` / `@After` | `@BeforeEach` / `@AfterEach` |
| 전체 초기화 | `@BeforeClass` / `@AfterClass` | `@BeforeAll` / `@AfterAll` |
| 실행 방식 | `@RunWith(SpringRunner.class)` | `@ExtendWith(SpringExtension.class)` |
| 예외 검증 | `@Test(expected = ...)` | `assertThatThrownBy()` |
| 타임아웃 | `@Test(timeout = 1000)` | `@Timeout` |
| 비활성화 | `@Ignore` | `@Disabled` |

- Spring Boot 2.2+ 부터 JUnit 5가 기본 — JUnit 4 의존성을 추가하지 않아도 된다.
- `@SpringBootTest`를 사용하면 `@ExtendWith(SpringExtension.class)`가 자동으로 포함된다.

## 테스트 클래스 구조

```java
@DisplayName("PostService 테스트")
class PostServiceTest {

    private PostService postService;
    private FakePostRepository fakePostRepository;

    @BeforeEach   // 각 테스트 메서드 실행 전 초기화
    void setUp() {
        fakePostRepository = new FakePostRepository();
        postService = new PostService(fakePostRepository);
    }

    @Test
    @DisplayName("게시글 생성 시 ID가 부여된다")
    void createPost_assignsId() {
        // Given
        CreatePostCommand command = new CreatePostCommand("제목", "내용");

        // When
        Post post = postService.create(command);

        // Then
        assertThat(post.getId()).isNotNull();
    }

    @Test
    @DisplayName("존재하지 않는 게시글 조회 시 예외가 발생한다")
    void findById_notFound_throwsException() {
        assertThatThrownBy(() -> postService.findById(999L))
            .isInstanceOf(ResourceNotFoundException.class);
    }
}
```

## @Nested로 테스트 그룹화

```java
@DisplayName("PostService 테스트")
class PostServiceTest {

    @Nested
    @DisplayName("게시글 생성")
    class Create {
        @Test
        @DisplayName("정상 생성 시 ID가 부여된다")
        void success() { ... }

        @Test
        @DisplayName("제목이 비어있으면 예외가 발생한다")
        void emptyTitle_throwsException() { ... }
    }

    @Nested
    @DisplayName("게시글 조회")
    class Find {
        @Test
        @DisplayName("정상 조회 시 게시글을 반환한다")
        void success() { ... }
    }
}
```

## 관련

- [[JUnit5]]
- [[TDD(Test Driven Development)]]
- [[BDD]]
- [[@BeforeEach]]
- [[@DisplayName]]
- [[@ParameterizedTest]]
- [[AssertJ]]
- [[Given-When-Then]]
