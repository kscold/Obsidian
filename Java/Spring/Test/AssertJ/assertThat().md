- [[AssertJ]]에서 assertThat() [[테스트 메서드(Test Method)]]는 주로 값을 비교하는 데 사용한다.
- 예를 들어, [[객체(Object)]]의 [[Java/필드(Field)|필드(Field)]]나 [[메서드(Method)]] 호출 결과와 기대값을 비교할 때 주로 사용된다.

- assertThat은 예외를 기대하지 않는다.
- 즉, 테스트 메서드 안에서 예외가 발생하면 해당 테스트는 실패한다.
- assertThat은 값을 검증할 때 사용한다.

## 메서드


### hasFieldOrPropertyWithValue()

```java
@Test
void no1() {
    String nickname = "name";
    int age = 10;

    Member member = createMember(nickname, age);

    assertThat(member.getNickname()).isEqualTo(nickname);
    assertThat(member.getAge()).isEqualTo(age);
}
```

- 위 방법은 가장 기본적인 방법이지만, 하나의 [[객체(Object)]]의 [[Java/필드(Field)|필드(Field)]]를 각각 검증할 땐 가독성이 급격히 떨어지는 문제가 발생한다.  
- 따라서 hasFieldOrPropertyWithValue()를 사용해 개선할 수 있다.

```java
@Test
void no1() {
    String nickname = "name";
    int age = 10;
	
    Member member = createMember(nickname, age);
	
    assertThat(member)
            .hasFieldOrPropertyWithValue("nickname", "nickname")
            .hasFieldOrPropertyWithValue("age", 30);
}
```

## 문제점

- assertThat 은 순차적으로 검증하고, 예상값과 다르면 로직을 바로 종료시킨다.
- 즉, 이후의 검증은 확인되지 않게된다.

```java
@Test
void no1() {
    String nickname = "name";
    int age = 10;
    boolean useAgreement = true;

    Member member = createMember(nickname, age, useAgreement);

    assertThat(member)
            .hasFieldOrPropertyWithValue("nickname", "nickname")
            .hasFieldOrPropertyWithValue("age", 30)
            .hasFieldOrPropertyWithValue("useAgreement", false);
}
```

- 위 코드는 첫번째, 세번째에서 검증을 실패해야하지만,  첫번째 밖에 찾아내지 못한다.

```null
java.lang.AssertionError: 

Expecting
to have a property or a field named "nickname" with value
  "nickname"

but value was:
  "name"

(static and synthetic fields are ignored)
```