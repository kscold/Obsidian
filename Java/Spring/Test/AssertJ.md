- AssertJ는 자바 테스트에서 유창하고 풍부한 assertions를 작성하는데 사용되는 오픈소스 라이브러리이다.


## [[테스트 메서드(Test Method)]]

### [[assertThat()]]

- assertThat은 주로 값을 비교하는 데 사용한다.
- 예를 들어, [[객체(Object)]]의 [[Java/필드(Field)|필드(Field)]]나 [[메서드(Method)]] 호출 결과와 기대값을 비교할 때 주로 사용된다.

- assertThat은 예외를 기대하지 않는다.
- 즉, 테스트 메서드 안에서 예외가 발생하면 해당 테스트는 실패한다.
- assertThat은 값을 검증할 때 사용한다.

```java
assertThat(actualValue).isEqualTo(expectedValue);
```

### assertThatCode 

- 예외가 발생하지 않을 때 테스트가 성공한다는 것이다.
- 에러가 발생하면 안되고 에러가 발생하면 실패한다.

- assertThatCode은 코드 블록을 실행하고 예외가 발생하는 경우에 대한 테스트를 수행한다.
- 예외가 발생하지 않는 경우 코드 블록이 예상대로 실행됐는지 확인한다.

- 즉, assertThatCode은 코드 블록 실행 중 예외 처리를 검증한다.(주로 예외 처리를 테스트할 때 사용된다.)

- doesNotThrowAnyException()를 붙이면 실행 중 예외가 발생하면 안된다.

```java
// 코드 블록 실행 중 예외가 발생하지 않아야 함 
assertThatCode(() -> someMethod()).doesNotThrowAnyException(); 

// 코드 블록 실행 중 특정 예외가 발생해야 함
assertThatCode(() -> someMethod()).isInstanceOf(SomeException.class)                                   .hasMessageContaining("expected message");`
```

- assertThatCode는 예외를 기대하고, 특정 조건을 만족하지 않을 경우에만 테스트를 실패시킨다.
- [[isinstance()]]

### assertThatThrownBy 

- 반드시 예외를 발생하는 코드가 존재해야 한다.
- 테스트하는 코드에서 예외가 발생하지 않으면, 즉각 에러가 터진다.
- 따라서 ThrowableAssert.ThrowingCallable이나 람다식에서 예외가 발생하는 코드가 발생해야 한다.

- 즉, AssertThatThrownBy는 에러가 발생해야 하는 상황을 테스트하는 코드인 것이다.
- 에러가 터져야 하는데 터지지 않으면 실패라고 생각하면 된다.
