- [[@ControllerAdvice]]와 @RestControllerAdvice Spring은 전역적으로 예외를 처리할 수 있는 @ControllerAdvice와 @RestControllerAdvice 어노테이션을 각각 Spring3.2, Spring4.3부터 제공하고 있다.

- 두 개의 차이는 @Controller와 RestController와 같은데, @RestControllerAdvice는 @ControllerAdvice와 달리 [[@ResponseBody]]가 붙어 있어 응답을 [[JSON(Java Script Object Notation)]]으로 내려준다는 점에서 다르다.


## @ControllerAdvice와 @RestControllerAdvice의 차이

```java
@Target(ElementType.TYPE) 
@Retention(RetentionPolicy.RUNTIME) 
@Documented 
@ControllerAdvice 
@ResponseBody // ResposeBody가 붙어있음
public @interface RestControllerAdvice {
    ... 
} 

@Target(ElementType.TYPE) 
@Retention(RetentionPolicy.RUNTIME) 
@Documented 
@Component 
public @interface ControllerAdvice {
    ... 
}
```
