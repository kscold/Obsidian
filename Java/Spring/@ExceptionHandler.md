- `@ExceptionHandler`는 **특정 예외를 처리하는 메서드**에 붙이는 [[어노테이션(Annotation)]]이다.
- [[@ControllerAdvice]] / [[@RestControllerAdvice]] 클래스 안에서 사용하면 전역 예외 처리기가 된다.
- 단일 컨트롤러 안에 붙이면 해당 컨트롤러 범위에서만 동작한다.

## 기본 사용

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(BusinessException.class)
    public ResponseEntity<ApiResponse<Void>> handleBusiness(BusinessException e) {
        ErrorCode ec = e.getErrorCode();
        return ResponseEntity
            .status(ec.getStatus())
            .body(ApiResponse.error(ec.getCode(), e.getMessage()));
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ApiResponse<Void>> handleUnexpected(Exception e) {
        return ResponseEntity
            .status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body(ApiResponse.error("C001", "서버 내부 오류"));
    }
}
```

## 여러 예외 타입을 하나의 메서드로 처리

```java
@ExceptionHandler({IllegalArgumentException.class, IllegalStateException.class})
public ResponseEntity<String> handleBadRequest(RuntimeException e) {
    return ResponseEntity.badRequest().body(e.getMessage());
}
```

## 처리 우선순위

- 같은 예외 타입에 대해 여러 핸들러가 있을 때: **더 구체적인 예외 타입**이 우선한다.
- `BusinessException`과 `Exception` 핸들러가 모두 있으면 `BusinessException` 핸들러가 먼저 실행된다.

## 컨트롤러 로컬 vs 전역

| 위치 | 적용 범위 |
| ---- | ---- |
| 컨트롤러 내부 | 해당 컨트롤러 요청만 |
| `@RestControllerAdvice` 클래스 | 모든 컨트롤러 (전역) |
| `@RestControllerAdvice(basePackages = "...")` | 특정 패키지 컨트롤러 |

## 관련

- [[@RestControllerAdvice]]
- [[@ControllerAdvice]]
- [[GlobalExceptionHandler]]
- [[BusinessException]]
