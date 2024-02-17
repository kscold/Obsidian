- 테스트 코드를 작성할 때, 중복되는 부분이 많은데 이렇게 여러 개의 [[테스트 메서드(Test Method)]]를 만드는 것은 관리하기도 가독성에도 좋지 않다.

- 물론 템플릿으로 빼는 방법도 있겠지만 오늘은 [[JUnit5]]에서 제공하는 @Parameterized [[어노테이션(Annotation)]]를 사용해서 해결할 수 있다.

- @ValueSource라는 추가적인 어노테이션을 사용하여 한번에 테스트를 할 수 있다.

## 예시

- 밑의 코드는 리팩토링 전의 코드이다.

```java
@Test 
@DisplayName("User 생성 name 2자 미만 예외처리")
void createUserException01() { 
	IllegalArgumentException e = assertThrows(IllegalArgumentException.class, () -> new User(VALID_EMAIL, "q", password));
	assertThat(e.getMessage()).isEqualTo(NAME_NOT_MATCH_MESSAGE); 
} 

@Test
@DisplayName("User 생성 name 10자 초과 예외처리") 
void createUserException02() { 
	IllegalArgumentException e = assertThrows(IllegalArgumentException.class, () -> new User(VALID_EMAIL, "qwerasdfzxcv", password)); 
	assertThat(e.getMessage()).isEqualTo(NAME_NOT_MATCH_MESSAGE); }
	
@Test
@DisplayName("User 생성 name 숫자 포함 예외처리")
void createUserException03() {
	IllegalArgumentException e = assertThrows(IllegalArgumentException.class, () -> new User(VALID_EMAIL, "qq23", password)); 
	assertThat(e.getMessage()).isEqualTo(NAME_NOT_MATCH_MESSAGE); 
}
```

- 밑의 코드는 @ParameterizedTest와 @ValueSource(strings = {})를 사용했을 때 코드이다.

```java
@ParameterizedTest 
@ValueSource(strings = {"q", "qwerasdfzxcv", "qq23"})
void createUserException(String name){ 
	IllegalArgumentException e = assertThrows(IllegalArgumentException.class, () -> new User(VALID_EMAIL, name, password)); 
	assertThat(e.getMessage()).isEqualTo(NAME_NOT_MATCH_MESSAGE); 
}
```

- 위의 코드처럼 strings로 설정해 놓으면 createUserException의 메서드의 매개변수이 name으로 매칭이 된다.