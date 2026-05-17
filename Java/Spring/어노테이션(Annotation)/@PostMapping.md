- `@PostMapping`은 HTTP `POST` 요청을 특정 핸들러 메서드에 매핑하는 [[어노테이션(Annotation)]]이다.
- `@RequestMapping(method = RequestMethod.POST)`의 축약형이다.
- 주로 **리소스 생성(Create)** 요청에 사용한다.

## HTTP 메서드 매핑 어노테이션 전체

| 어노테이션 | HTTP 메서드 | 주요 용도 |
| ---- | ---- | ---- |
| `@GetMapping` | GET | 조회 (읽기) |
| `@PostMapping` | POST | 생성 |
| `@PutMapping` | PUT | 전체 수정 |
| `@PatchMapping` | PATCH | 부분 수정 |
| `@DeleteMapping` | DELETE | 삭제 |

## 기본 사용

```java
@RestController
@RequestMapping("/api/posts")
@RequiredArgsConstructor
public class PostController {

    private final PostUseCase postUseCase;

    // POST /api/posts
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)   // 201 반환
    public PostResponse create(@RequestBody @Valid CreatePostRequest request) {
        return postUseCase.create(request.toCommand());
    }

    // POST /api/posts/{id}/publish
    @PostMapping("/{id}/publish")
    public PostResponse publish(@PathVariable String id) {
        return postUseCase.publish(id);
    }
}
```

## RequestBody와 함께 사용

```java
@PostMapping("/login")
public ResponseEntity<LoginResponse> login(@RequestBody LoginRequest request) {
    LoginResponse response = authUseCase.login(request);
    return ResponseEntity.ok(response);
}
```

## 응답 상태 코드

- 생성 성공 시 `201 Created`를 반환하는 것이 REST 관례.
- `@ResponseStatus(HttpStatus.CREATED)` 또는 `ResponseEntity.created(uri).body(data)` 사용.

```java
@PostMapping
public ResponseEntity<PostResponse> create(@RequestBody CreatePostRequest req) {
    PostResponse created = postUseCase.create(req.toCommand());
    URI location = URI.create("/api/posts/" + created.getId());
    return ResponseEntity.created(location).body(created);  // 201 + Location 헤더
}
```

## 관련

- [[@RequestMapping]]
- [[@GetMapping]]
- [[@RequestBody]]
- [[@ResponseBody]]
- [[컨트롤러(Controller)]]
- [[HTTP]]
- [[HTTP 상태 코드(HTTP Status)]]
