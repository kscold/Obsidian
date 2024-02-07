- List.of()는 자바 9에서 도입된 메서드이다.
- Arrays.of()와는 대조적으로, 이 메소드는 변경불가한(unmodifiable) list 객체를 생성한다.

- 결론적으로 Arrays.asList()는 Array로부터 생성된 list 안의 element의 변경이 원본 array에도 반영될 수 있기 때문에 찾기 어려운 버그를 발생시키는 side effects를 초래할 수 있다.

- 따라서 자바 9 이상을 사용하는 환경이라면 되도록 List.of()를 사용하도록 하고, 두 메소드의 차이점도 확실히 알고 사용해야한다.

## 예시

```java
@DisplayName("List.of() 사용방법")
@Test
void usageOfListOf() {
    Integer[] array = new Integer[]{1, 2, 3};
    List<Integer> list = List.of(array);
    assertThat(list).containsExactly(1, 2, 3)
}
```

## Arrays.asList()와의 차이점

- [[배열(Array)]].[[Arrays.asList()]]와 대조되는 List.of()의 특징은 다음과 같다.

- 입력된 array에 대해 불변의(immutable) list를 반환한다.
- 즉, 원본 array의 변경이 생성된 list에 반영되지 않는다.
- 인풋으로 null이 허용되지 않는다.(입력 시 NullPointerException 발생한다.)

### 불변 리스트(Immutable List)

- 주요 차이점은 List.of()는 입력된 array에 대해 불변의(immutable) list를 반환하여, 원본 [[배열(Array)]]의 변경이 생성된 [[List]]에 반영되지 않는다는 것이다.
- 또한 List.of()로 생성된 list의 각 element는 변경될 수 없다.
- 변경하려하면 UnsupportedOperationException이 발생한다.

```java
@DisplayName("Immutable List -> 원본 array의 변경이 list에 반영되지 않고, list의 각 요소 변경 시 예외 발생")
@Test
void immutableList() {
    Integer[] array = new Integer[]{1, 2, 3};
    List<Integer> list = List.of(array);
    array[0] = 1000;
    assertThat(list.get(0)).isEqualTo(1);
    assertThrows(UnsupportedOperationException.class, () -> list.set(1, 4));
}
```

### Null

- List.of()는 null 값을 입력으로 허용하지 않는다.
- null이 입력되면 NullPointerException이 발생한다.


```java
@DisplayName("null 값을 허용하지 않음")
@Test
void notAllowNullValue() {
    assertThrows(NullPointerException.class, () -> List.of(1, null, 3));
}
```
