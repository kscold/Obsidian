- `@Controller`는 **해당 [[클래스(Class)]]가 [[MVC(Mode View Controller)]] 패턴의 [[컨트롤러(Controller)]]임을 선언**하는 [[어노테이션(Annotation)]]이다.
- `@Component`의 특화형으로, [[스프링 컨테이너(Spring Container)]]가 [[Bean]]으로 등록하고 클라이언트 요청을 해당 컨트롤러에 매핑한다.
- 메서드에 `@RequestMapping` 계열 어노테이션을 붙여 URL 라우팅을 처리한다.
- [[뷰(View)]] 이름을 반환하는 전통적인 방식과, `@ResponseBody`를 추가해 JSON을 직접 반환하는 방식 모두 지원한다.

## @Controller vs @RestController

| 어노테이션 | 반환 방식 | 주 용도 |
| ---- | ---- | ---- |
| `@Controller` | [[뷰(View)]] 이름 (Thymeleaf, JSP) | 서버 사이드 렌더링 |
| `@RestController` | JSON/XML (HTTP 응답 바디) | REST API |

```java
// @RestController = @Controller + @ResponseBody
@RestController
public class PostController {
    @GetMapping("/api/posts/{id}")
    public PostResponse getPost(@PathVariable Long id) {
        return postUseCase.findById(id);   // JSON으로 직렬화
    }
}

// @Controller — 뷰 이름 반환
@Controller
public class PageController {
    @GetMapping("/posts/{id}")
    public String postDetail(@PathVariable Long id, Model model) {
        model.addAttribute("post", postUseCase.findById(id));
        return "post/detail";   // templates/post/detail.html 렌더링
    }
}
```

## 요청 매핑 어노테이션

```java
@RestController
@RequestMapping("/api/posts")   // 공통 prefix
@RequiredArgsConstructor
public class PostController {

    private final PostUseCase postUseCase;

    @GetMapping("/{id}")          // GET /api/posts/{id}
    public PostResponse getPost(@PathVariable Long id) {
        return postUseCase.findById(id);
    }

    @PostMapping                  // POST /api/posts
    @ResponseStatus(HttpStatus.CREATED)
    public PostResponse createPost(@RequestBody @Valid CreatePostRequest request) {
        return postUseCase.create(request.toCommand());
    }

    @PutMapping("/{id}")          // PUT /api/posts/{id}
    public PostResponse updatePost(@PathVariable Long id,
                                   @RequestBody @Valid UpdatePostRequest request) {
        return postUseCase.update(id, request.toCommand());
    }

    @DeleteMapping("/{id}")       // DELETE /api/posts/{id}
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void deletePost(@PathVariable Long id) {
        postUseCase.delete(id);
    }
}
```

## 주요 파라미터 바인딩

```java
@GetMapping("/posts")
public List<PostResponse> getPosts(
    @RequestParam(defaultValue = "0") int page,        // 쿼리 파라미터 (?page=0)
    @RequestParam(required = false) String keyword,    // 선택적 쿼리 파라미터
    @RequestHeader("Authorization") String token       // 요청 헤더
) { ... }

@PostMapping("/posts")
public PostResponse createPost(
    @RequestBody @Valid CreatePostRequest request,      // 요청 바디 (JSON → 객체)
    @AuthenticationPrincipal UserDetails userDetails   // Spring Security 현재 사용자
) { ... }
```

## 헥사고날 아키텍처에서의 위치

```java
// adapter/in/web/PostController.java
@RestController
@RequestMapping("/api/posts")
@RequiredArgsConstructor
public class PostController {

    private final PostUseCase postUseCase;   // 구체 클래스 주입 금지, 인터페이스만 주입

    @GetMapping("/{id}")
    public ApiResponse<PostResponse> getPost(@PathVariable Long id) {
        PostResponse response = postUseCase.findById(id);
        return ApiResponse.success(response);
    }
}
```

- `PostApplicationService` 직접 주입 금지 — 반드시 `PostUseCase` 인터페이스 주입.
- HTTP 요청/응답 변환만 담당, [[비즈니스 로직(Business Logic)]]은 [[서비스(Service)]]에 위임.

## 관련

- [[MVC(Mode View Controller)]]
- [[컨트롤러(Controller)]]
- [[뷰(View)]]
- [[어노테이션(Annotation)]]
- [[스프링 컨테이너(Spring Container)]]
- [[Bean]]
- [[DI(Dependency Injection)]]
- [[@Service]]
- [[비즈니스 로직(Business Logic)]]
