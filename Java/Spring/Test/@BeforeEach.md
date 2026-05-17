- `@BeforeEach`는 [[JUnit5]]에서 **각 테스트 메서드 실행 전에 공통 준비 코드를 실행**하는 [[어노테이션(Annotation)]]이다.
- 테스트마다 새로운 상태를 만들어 테스트 간 격리를 보장한다.
- JUnit 4의 `@Before`에 해당한다.

## 기본 사용

```java
@DisplayName("PostService 테스트")
class PostServiceTest {

    private PostService postService;
    private FakePostRepository postRepository;

    @BeforeEach
    void setUp() {
        postRepository = new FakePostRepository();   // 매 테스트마다 새 레포지토리
        postService = new PostService(postRepository);
    }

    @Test
    void createPost_success() {
        // Given - BeforeEach에서 이미 준비된 postService 사용
        CreatePostCommand command = new CreatePostCommand("제목", "내용");

        // When
        Post post = postService.create(command);

        // Then
        assertThat(post.getTitle()).isEqualTo("제목");
    }
}
```

## 생명주기 어노테이션 전체

| 어노테이션 | 실행 시점 | static 필요 |
| ---- | ---- | ---- |
| `@BeforeAll` | 클래스 전체에서 1회, 모든 테스트 전 | O (static) |
| `@BeforeEach` | 각 테스트 메서드 실행 전 | X |
| `@AfterEach` | 각 테스트 메서드 실행 후 | X |
| `@AfterAll` | 클래스 전체에서 1회, 모든 테스트 후 | O (static) |

```java
@BeforeAll
static void initAll() {
    // DB 연결 등 비용이 큰 초기화 (1회만)
}

@BeforeEach
void setUp() {
    // 각 테스트마다 새 객체 생성
}

@AfterEach
void tearDown() {
    // 각 테스트 후 정리 (파일 삭제, 상태 초기화 등)
}

@AfterAll
static void cleanUpAll() {
    // 전체 종료 후 정리 (DB 연결 해제 등)
}
```

## @SpringBootTest와 함께 사용

```java
@SpringBootTest
class PostControllerIntegrationTest {

    @Autowired
    private PostRepository postRepository;

    @BeforeEach
    void setUp() {
        postRepository.deleteAll();   // 매 테스트 전 DB 초기화
    }
}
```

## 관련

- [[JUnit5]]
- [[@Test]]
- [[@DisplayName]]
- [[TDD(Test Driven Development)]]
- [[BDD]]
- [[Given-When-Then]]
