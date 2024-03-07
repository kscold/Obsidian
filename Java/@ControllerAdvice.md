- ControllerAdvice는 여러 컨트롤러에 대해 전역적으로 ExceptionHandler를 적용해준다.
- @ControllerAdvice [[어노테이션(Annotation)]]에는 [[@Component]] [[어노테이션(Annotation)]]이 있어서 ControllerAdvice가 선언된 클래스는 스프링 빈([[Bean]])으로 등록된다. 

- 그러므로 우리는 다음과 같이 전역적으로 에러를 핸들링하는 [[클래스(Class)]]를 만들어 [[어노테이션(Annotation)]]을 붙여줌으로써 에러 처리를 위임할 수 있다.

- @ControllerAdvice를 이용하면 하나의 [[클래스(Class)]]로 모든 [[컨트롤러(Controller)]]에 대해 전역적으로 예외 처리가 가능하다.
- 직접 정의한 에러 응답을 일관성있게 클라이언트에게 내려줄 수 있다.
- 별도의 try-catch문이 없어 코드의 가독성이 높아진다.

- 이러한 이유로 [[API(Application Programming Interface)]]에 의한 예외 처리를 할 때에는 ControllerAdvice를 이용하면 평가된다. 
- 하지만 ControllerAdvice를 사용할 때에는 항상 다음의 내용들을 주의해야 한다. 

- 여러 ControllerAdvice가 있을 때 @Order 어노테이션으로 순서를 지정하지 않는다면 Spring은 ControllerAdvice를 임의의 순서로 에러를 처리할 수 있다.
- 그러므로 일관된 예외 응답을 위해서는 이러한 점에 주의해야 한다.

- 한 프로젝트당 하나의 ControllerAdvice만 관리하는 것이 좋다.
- 만약 여러 ControllerAdvice가 필요하다면 basePackages나 [[어노테이션(Annotation)]] 등을 지정해야 한다.
- 직접 구현한 Exception 클래스들은 한 공간에서 관리한다.

## 예시


```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(NoSuchElementFoundException.class)
    protected ResponseEntity<?> handleIllegalArgumentException(NoSuchElementFoundException e) {
        final ErrorResponse errorResponse = ErrorResponse.builder()
                .code("Item Not Found")
                .message(e.getMessage()).build();

        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(errorResponse);
    }
}
```

