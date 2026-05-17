- Given-When-Then은 **단위/통합 테스트에서 코드 가독성을 높이는 테스트 구조화 패턴**이다.
- [[BDD]](Behavior Driven Development)에서 비롯되었으며 [[JUnit5]] + [[AssertJ]]와 자연스럽게 어울린다.
- 각 단계를 명확히 구분하면 테스트의 의도를 한눈에 파악할 수 있다.

## 구조

| 단계 | 역할 | 설명 |
| ---- | ---- | ---- |
| **Given** (준비) | 사전 조건 설정 | 테스트 환경과 데이터 준비, `@BeforeEach` 초기화 |
| **When** (실행) | 행동 수행 | 테스트하려는 메서드 실행 |
| **Then** (검증) | 결과 확인 | `assertThat`으로 예상 결과와 실제 결과 비교 |

## 서비스 단위 테스트 예시

```java
@DisplayName("PostService 테스트")
class PostServiceTest {

    private PostService postService;
    private FakePostRepository postRepository;

    @BeforeEach
    void setUp() {
        postRepository = new FakePostRepository();
        postService = new PostService(postRepository);
    }

    @Test
    @DisplayName("게시글을 생성하면 ID가 부여된 게시글을 반환한다")
    void createPost_success() {
        // Given
        CreatePostCommand command = new CreatePostCommand("제목", "내용", "DRAFT");

        // When
        Post post = postService.create(command);

        // Then
        assertThat(post.getId()).isNotNull();
        assertThat(post.getTitle()).isEqualTo("제목");
        assertThat(post.getStatus()).isEqualTo("DRAFT");
    }

    @Test
    @DisplayName("존재하지 않는 게시글 조회 시 ResourceNotFoundException이 발생한다")
    void findById_notFound_throwsException() {
        // Given
        Long nonExistentId = 999L;

        // When & Then (예외 검증)
        assertThatThrownBy(() -> postService.findById(nonExistentId))
            .isInstanceOf(ResourceNotFoundException.class)
            .hasMessageContaining("999");
    }
}
```

## 컨트롤러 테스트 예시 (MockMvc)

```java
@WebMvcTest(PostController.class)
class PostControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private PostUseCase postUseCase;

    @Test
    @DisplayName("[GET] /api/posts/{id} - 정상 조회 시 200 OK")
    void getPost_success() throws Exception {
        // Given
        long postId = 1L;
        PostResponse response = new PostResponse(postId, "제목", "PUBLISHED");
        given(postUseCase.findById(postId)).willReturn(response);

        // When & Then
        mockMvc.perform(get("/api/posts/{id}", postId)
                .contentType(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.id").value(postId))
            .andExpect(jsonPath("$.title").value("제목"))
            .andDo(print());
    }
}
```

## When & Then 합치기

- 예외 검증처럼 실행과 검증이 같은 라인에 있을 때는 주석으로 묶는다.

```java
@Test
void divideByZero_throwsArithmeticException() {
    // Given
    int dividend = 10;
    int divisor = 0;

    // When & Then
    assertThatThrownBy(() -> calculator.divide(dividend, divisor))
        .isInstanceOf(ArithmeticException.class);
}
```

## 주석 없이 공백으로 구분하는 스타일

```java
@Test
void createPost_success() {
    CreatePostCommand command = new CreatePostCommand("제목", "내용");

    Post post = postService.create(command);

    assertThat(post.getTitle()).isEqualTo("제목");
}
```

- 주석(`// Given`)을 쓰지 않아도 **빈 줄**로 구분하면 가독성이 유지된다.

## 관련

- [[BDD]]
- [[JUnit5]]
- [[AssertJ]]
- [[@Test]]
- [[@BeforeEach]]
- [[TDD(Test Driven Development)]]
- [[MockMvc]]
