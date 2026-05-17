- `@MockBean`은 **Spring Boot Test에서 특정 [[Bean]]을 가짜(Mock) [[객체(Object)]]로 대체**하는 [[어노테이션(Annotation)]]이다.
- Mockito의 `@Mock`과 달리 [[스프링 컨테이너(Spring Container)]]에 실제 Bean으로 등록되어 [[DI(Dependency Injection)]]가 동작한다.
- `@WebMvcTest`나 `@SpringBootTest`와 함께 사용하며, 테스트하려는 계층의 의존성을 격리할 때 사용한다.

## @Mock vs @MockBean 차이

| 항목 | @Mock (Mockito) | @MockBean (Spring) |
| ---- | ---- | ---- |
| 등록 위치 | Spring 컨텍스트 밖 | Spring 컨텍스트 안 |
| DI 동작 | X | O |
| 사용 환경 | 순수 단위 테스트 | @WebMvcTest, @SpringBootTest |
| 사용 어노테이션 | @ExtendWith(MockitoExtension.class) | @WebMvcTest, @SpringBootTest |

## @WebMvcTest와 함께 사용 (컨트롤러 테스트)

```java
@WebMvcTest(PostController.class)
class PostControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean   // 실제 PostUseCase 대신 Mock 객체가 컨텍스트에 등록됨
    private PostUseCase postUseCase;

    @Test
    @DisplayName("게시글 조회 API 테스트")
    void getPost_success() throws Exception {
        // Mock 동작 정의
        given(postUseCase.findById(1L))
            .willReturn(new PostResponse(1L, "제목", "PUBLISHED"));

        // When & Then
        mockMvc.perform(get("/api/posts/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.title").value("제목"));
    }

    @Test
    @DisplayName("서비스 예외 → 404 응답 매핑 테스트")
    void getPost_notFound_returns404() throws Exception {
        // 예외를 던지도록 Mock 설정
        given(postUseCase.findById(999L))
            .willThrow(new ResourceNotFoundException("Post not found: 999"));

        mockMvc.perform(get("/api/posts/999"))
            .andExpect(status().isNotFound());
    }
}
```

## @SpringBootTest와 함께 사용 (일부 Bean만 Mock)

```java
@SpringBootTest
class PostServiceIntegrationTest {

    @Autowired
    private PostService postService;

    @MockBean   // 외부 API 호출 같은 특정 의존성만 Mock
    private LinkScrapingAdapter linkScrapingAdapter;

    @Test
    void createPost_withMockedLinkScraping() {
        given(linkScrapingAdapter.scrape("https://example.com"))
            .willReturn(new LinkMetadata("제목", "설명", "thumbnail.png"));

        // 실제 서비스 로직 + Mock 외부 API 조합으로 테스트
        PostResponse result = postService.createWithLink("https://example.com");
        assertThat(result.getTitle()).isEqualTo("제목");
    }
}
```

## given / when / then (Mockito BDD 스타일)

```java
import static org.mockito.BDDMockito.*;

// BDD 스타일 (권장)
given(postUseCase.findById(1L)).willReturn(response);       // 반환값 설정
given(postUseCase.findById(999L)).willThrow(new RuntimeException());  // 예외 설정

// 인자 매처
given(postUseCase.create(any(CreatePostCommand.class))).willReturn(response);
given(postUseCase.findByTitle(anyString())).willReturn(List.of());

// 검증 — 해당 메서드가 실제로 호출됐는지
then(postUseCase).should(times(1)).findById(1L);
then(postUseCase).should(never()).deleteById(anyLong());
```

## @SpyBean — 실제 Bean + 일부만 Mock

```java
@SpringBootTest
class PostServiceTest {

    @SpyBean   // 실제 Bean이지만 일부 메서드만 Mock 가능
    private EmailService emailService;

    @Test
    void createPost_emailSentOnce() {
        // 이메일 전송 메서드만 Mock (실제 실행 방지)
        doNothing().when(emailService).sendNotification(anyString());

        postService.create(command);

        // 이메일이 1번 호출됐는지 검증
        verify(emailService, times(1)).sendNotification(anyString());
    }
}
```

## 관련

- [[JUnit5]]
- [[MockMvc]]
- [[@SpringBootTest]]
- [[BDD]]
- [[AssertJ]]
- [[DI(Dependency Injection)]]
