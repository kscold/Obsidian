- `@SpringBootTest`는 **전체 [[스프링 컨테이너(Spring Container)]]를 로드하여 통합 테스트를 수행**하는 [[어노테이션(Annotation)]]이다.
- 실제 [[Bean]]을 모두 띄워서 실제 환경에 가장 가까운 테스트를 할 수 있다.
- 반면 `@WebMvcTest` 같은 슬라이스 테스트보다 **느리다** — 컨텍스트 로드 시간이 크다.
- [[MockMvc]], [[JUnit5]], [[AssertJ]]와 함께 사용한다.

## 기본 사용

```java
@SpringBootTest
@AutoConfigureMockMvc   // MockMvc 자동 설정
@Transactional          // 각 테스트 후 DB 롤백
class PostApiIntegrationTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private PostRepository postRepository;

    @BeforeEach
    void setUp() {
        postRepository.deleteAll();   // 매 테스트 전 DB 초기화
    }

    @Test
    @DisplayName("게시글을 생성하면 DB에 저장된다")
    void createPost_savedToDatabase() throws Exception {
        // Given
        String requestBody = """
            {"title": "제목", "content": "내용"}
            """;

        // When
        mockMvc.perform(post("/api/posts")
                .contentType(MediaType.APPLICATION_JSON)
                .content(requestBody))
            .andExpect(status().isCreated());

        // Then
        assertThat(postRepository.findAll()).hasSize(1);
    }
}
```

## webEnvironment 옵션

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.MOCK)      // 기본값 — MockMvc
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT) // 실제 서버, 랜덤 포트
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT) // 실제 서버, 고정 포트
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.NONE)        // 웹 환경 없음
```

| 옵션 | 설명 | 사용 상황 |
| ---- | ---- | ---- |
| `MOCK` | 서블릿 없이 MockMvc | 컨트롤러 통합 테스트 |
| `RANDOM_PORT` | 실제 서버, 랜덤 포트 | `TestRestTemplate` 사용 |
| `DEFINED_PORT` | 실제 서버, `server.port` 사용 | E2E 테스트 |
| `NONE` | 웹 없음 | 서비스/리포지토리 통합 테스트 |

## RANDOM_PORT — TestRestTemplate

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class PostE2ETest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    void createPost_returnsCreated() {
        CreatePostRequest request = new CreatePostRequest("제목", "내용");
        ResponseEntity<PostResponse> response = restTemplate
            .postForEntity("/api/posts", request, PostResponse.class);

        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.CREATED);
        assertThat(response.getBody().getTitle()).isEqualTo("제목");
    }
}
```

## 테스트 DB 격리 방법

```yaml
# application-test.yml (test 환경 전용 설정)
spring:
  datasource:
    url: jdbc:h2:mem:testdb   # 인메모리 H2 DB 사용
  jpa:
    hibernate:
      ddl-auto: create-drop    # 테스트 시 스키마 자동 생성/제거
```

```java
@SpringBootTest
@ActiveProfiles("test")   // application-test.yml 적용
class PostServiceIntegrationTest {
    // H2 인메모리 DB로 격리된 테스트
}
```

## @SpringBootTest vs @WebMvcTest

| 항목 | @SpringBootTest | @WebMvcTest |
| ---- | ---- | ---- |
| 로드 범위 | 전체 컨텍스트 | 웹 레이어만 |
| 속도 | 느림 | 빠름 |
| 실제 DB | O (H2/실제 DB) | X |
| @MockBean 필요 | 필요 시만 | 서비스 레이어 |
| 용도 | 통합/E2E 테스트 | 컨트롤러 단위 테스트 |

## 관련

- [[JUnit5]]
- [[MockMvc]]
- [[AssertJ]]
- [[@MockBean]]
- [[@BeforeEach]]
- [[DI(Dependency Injection)]]
- [[스프링 컨테이너(Spring Container)]]
