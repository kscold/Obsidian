
- [[TDD(Test Driven Development)]]를 짤 때, Junit5에서는 기본적으로 클래스명, 메서드명 그대로 테스트 이름을 보여준다.
- 따라서 camelCase 혹은 snake_case 테스트 결과가 한눈에 들어오지는 않는다.

- @DisplayName을 [[어노테이션(Annotation)]]을 이용하면 테스트 [[클래스(Class)]] 혹은 테스트 [[메서드(Method)]]의 이름을 지정할 수 있다.


## 문법

```java
@DisplayName("비밀번호가 최소 8자 이상, 12자 이하면 예외가 발생하지 않는다.") 
```