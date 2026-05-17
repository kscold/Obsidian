- [[컨트롤러(Controller)]]는 **[[MVC(Mode View Controller)]] 패턴에서 클라이언트 요청을 받아 [[서비스(Service)]] 레이어에 처리를 위임하고 결과를 응답**하는 계층이다.
- [[모델(Model)]]과 [[뷰(View)]]를 연결하는 중간 다리 역할을 한다.
- [[모델(Model)]] 또는 [[뷰(View)]]의 변경을 인지하고 대응하며, 직접 [[비즈니스 로직(Business Logic)]]을 처리하지 않는다.

## MVC에서의 역할

```mermaid
flowchart LR
    Client["클라이언트"] -->|HTTP 요청| Controller["Controller<br/>@RestController"]
    Controller -->|위임| Service["Service<br/>@Service"]
    Service -->|결과 반환| Controller
    Controller -->|HTTP 응답 (JSON)| Client
```

- 클라이언트 요청 수신 → 파라미터 바인딩 → [[서비스(Service)]] 호출 → 응답 변환의 흐름.
- [[비즈니스 로직(Business Logic)]] 직접 구현 금지 — [[서비스(Service)]]에만 위임.

## 비유를 통한 이해

- 클라이언트가 손님이면 컨트롤러는 주문을 받고 음식이 나오면 서빙하는 웨이터에 해당한다.
- 주방([[서비스(Service)]])에서 어떻게 음식을 만드는지([[비즈니스 로직(Business Logic)]]) 알 필요 없이, 주문 전달과 서빙만 담당한다.

## Spring에서의 구현

```java
// [[@Controller]] = 뷰 이름 반환 (서버 사이드 렌더링)
// @RestController = JSON 직접 반환 (REST API)
@RestController
@RequestMapping("/api/posts")
@RequiredArgsConstructor
public class PostController {

    private final PostUseCase postUseCase;   // 서비스 인터페이스 주입

    @GetMapping("/{id}")
    public ApiResponse<PostResponse> getPost(@PathVariable Long id) {
        return ApiResponse.success(postUseCase.findById(id));
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public ApiResponse<PostResponse> createPost(@RequestBody @Valid CreatePostRequest request) {
        return ApiResponse.success(postUseCase.create(request.toCommand()));
    }
}
```

## 컨트롤러의 책임 범위

| 해야 할 것 | 하면 안 되는 것 |
| ---- | ---- |
| HTTP 요청 파라미터 추출 | 비즈니스 로직 직접 처리 |
| 요청 객체 → Command 변환 | DB 직접 접근 |
| 서비스 메서드 호출 | 복잡한 조건 분기 |
| 결과 → Response 변환 | 외부 API 직접 호출 |
| HTTP 상태 코드 설정 | 다른 서비스 여러 개 직접 조율 |

## 헥사고날 아키텍처에서의 위치

- `adapter/in/web/` 패키지에 위치 — 인바운드 어댑터.
- `UseCase` 인터페이스(`application/port/in`)만 의존 — 구체 서비스 클래스 직접 참조 금지.
- 요청 DTO(`*Request`) → Command 변환, 도메인 결과 → 응답 DTO(`*Response`) 변환이 주 역할.

## 관련

- [[MVC(Mode View Controller)]]
- [[모델(Model)]]
- [[뷰(View)]]
- [[@Controller]]
- [[서비스(Service)]]
- [[비즈니스 로직(Business Logic)]]
- [[DI(Dependency Injection)]]
- [[Bean]]
