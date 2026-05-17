- GlobalExceptionHandler는 [[@RestControllerAdvice]]가 붙은 [[클래스(Class)]]로, **모든 [[컨트롤러(Controller)]]에서 발생하는 예외를 한 곳에서 일관된 형식으로 변환**하는 전역 예외 처리기이다.
- [[BusinessException]] 계층 + [[@ExceptionHandler]] 매핑으로 도메인 예외를 깔끔한 [[HTTP 상태 코드(HTTP Status)]]와 JSON 응답으로 매핑한다.

- 컨트롤러마다 try-catch를 흩어놓지 않고 중앙집중식으로 관리한다.
- API 응답의 일관성을 보장하고, 로깅 정책도 한 곳에서 결정한다.

## 기본 구조

```java
@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(BusinessException.class)
    protected ResponseEntity<ApiResponse<Void>> handleBusinessException(BusinessException e) {
        log.warn("BusinessException: code={}, message={}",
            e.getErrorCode().getCode(), e.getMessage());

        ErrorCode errorCode = e.getErrorCode();
        return new ResponseEntity<>(
            ApiResponse.error(errorCode.getCode(), e.getMessage()),
            errorCode.getStatus()
        );
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    protected ResponseEntity<ApiResponse<Void>> handleValidation(MethodArgumentNotValidException e) {
        String message = e.getBindingResult().getFieldErrors().stream()
            .map(FieldError::getDefaultMessage)
            .collect(Collectors.joining(", "));
        return new ResponseEntity<>(
            ApiResponse.error(ErrorCode.INVALID_INPUT_VALUE.getCode(), message),
            HttpStatus.BAD_REQUEST
        );
    }

    @ExceptionHandler(Exception.class)
    protected ResponseEntity<ApiResponse<Void>> handleUnexpected(Exception e) {
        log.error("Unexpected exception", e);
        return new ResponseEntity<>(
            ApiResponse.error(ErrorCode.INTERNAL_SERVER_ERROR.getCode(),
                "서버 내부 오류가 발생했습니다"),
            HttpStatus.INTERNAL_SERVER_ERROR
        );
    }
}
```

## 처리해야 할 예외 카테고리

| 카테고리 | 예외 | HTTP 상태 | 로그 레벨 |
| ---- | ---- | ---- | ---- |
| 도메인 비즈니스 | [[BusinessException]] 계층 | ErrorCode가 결정 | warn |
| 검증 실패 | `MethodArgumentNotValidException`, `BindException` | 400 | warn |
| 타입 불일치 | `MethodArgumentTypeMismatchException` | 400 | warn |
| HTTP 메서드 불일치 | `HttpRequestMethodNotSupportedException` | 405 | warn |
| 인증 실패 | `AuthenticationException` | 401 | warn |
| 인가 실패 | `AccessDeniedException` | 403 | warn |
| 리소스 없음 | `NoResourceFoundException` (정적) | 404 | debug |
| 그 외 | `Exception` | 500 | **error + 스택트레이스** |

## 처리 흐름

```mermaid
flowchart TD
    A[Controller에서 예외 발생] --> B{어떤 예외?}
    B -->|BusinessException| C[ErrorCode 기반<br/>HTTP status + 메시지]
    B -->|@Valid 실패| D[필드 메시지 결합<br/>400 Bad Request]
    B -->|AuthenticationException| E[401 Unauthorized]
    B -->|AccessDeniedException| F[403 Forbidden]
    B -->|Exception| G[500 Internal<br/>+ error 로그]

    C --> R[ApiResponse JSON]
    D --> R
    E --> R
    F --> R
    G --> R
    R --> Client
```

## 응답 형식 일관화

- 모든 에러 응답은 동일한 `ApiResponse` 래퍼로 감싼다.

```json
{
  "success": false,
  "code": "POST_NOT_FOUND",
  "message": "해당 게시글을 찾을 수 없습니다",
  "data": null
}
```

## 로깅 전략

- **warn**: 비즈니스 예외(예측된 흐름). 스택트레이스 없이 메시지만.
- **error**: 예상치 못한 예외. 스택트레이스 포함.
- **debug**: 노이즈가 큰 케이스(예: socket.io의 404). 운영에서는 안 보이게.
- 토큰/비밀번호 등 민감 정보는 로그에 절대 남기지 않는다.

## 컨트롤러 어드바이스의 적용 범위

- `@RestControllerAdvice`(annotations = ...)로 특정 어노테이션을 가진 컨트롤러만.
- `@RestControllerAdvice`(basePackages = ...)로 특정 패키지만.
- 기본값은 전체 컨트롤러.

```java
@RestControllerAdvice(basePackages = "com.kscold.blog.admin")
public class AdminExceptionHandler { ... }
```

## 처리 우선순위

- 같은 예외 타입에 대해 여러 핸들러가 있으면 **더 구체적인 타입**이 우선.
- 여러 `@RestControllerAdvice`가 있으면 `@Order`로 우선순위 지정.

## 안티패턴

- **모든 예외를 catch해서 500으로 응답** → ErrorCode 매핑 못 함. 도메인 예외 따로 처리.
- **검증 실패도 500** → `MethodArgumentNotValidException`은 400으로.
- **민감 정보 응답에 포함** → 스택트레이스/내부 SQL을 클라이언트로 보내지 말 것.
- **handler에서 또 예외 던지기** → 무한 루프 위험. 내부에서 try-catch.

## 관련

- [[@RestControllerAdvice]]
- [[@ControllerAdvice]]
- [[BusinessException]]
- [[HTTP 상태 코드(HTTP Status)]]
- [[@ExceptionHandler]]
