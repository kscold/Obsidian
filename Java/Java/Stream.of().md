

## Java에서 Stream.of()와 Arrays.stream() 메서드의 차이점 

- 그만큼 Stream.of() 그리고 Arrays.stream() 지정된 [[배열(Array)]]에서 순차 [[스트림(Stream)]]을 만드는 데 일반적으로 사용되는 두 가지 방법이다.
- 이 두 가지 방법 모두 다음을 반환한다.
- `Stream<T>` 기본이 아닌 유형으로 호출될 때 T

- 밑의 코드처럼 둘 다 Stream.of() 그리고 Arrays.stream()가 `Stream<Integer>` , 즉 Integer [[배열(Array)]]을 호출 할 때 예시이다.

```java
Integer[] array = { 1, 2, 3, 4, 5 };

Stream.of(array) // return Stream<Integer>
	.forEach(System.out::println); // 1, 2, 3, 4, 5

Integer[] array = { 1, 2, 3, 4, 5 };

Arrays.stream(array)                        // return Stream<Integer>

    .forEach(System.out::println);      // 인쇄물 1, 2, 3, …
```

- Stream.of() [[메서드(Method)]]는 단순히 호출 `Arrays.stream()` 의 소스 코드에서 명백한 비 기본 유형에 대한 방법 `Stream.of()` 방법:

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10<br><br>11<br><br>12|/**<br><br>* 요소가 지정된 값인 순차적으로 정렬된 스트림을 반환합니다.<br><br>*<br><br>* @param `<T>` 스트림 요소의 유형<br><br>* @param `values` 새 스트림의 요소<br><br>* @return 새 스트림<br><br>*/<br><br>@SafeVarargs<br><br>@SuppressWarnings("varargs") // 어레이에서 스트림을 생성하는 것은 안전합니다.<br><br>public static<T> Stream<T> of(T... values) {<br><br>    return Arrays.stream(values);<br><br>}|

   
만약에 `Stream.of()` 그냥 래퍼입니다 `Arrays.stream()` 메소드, 왜 Java에 포함되어 있습니까? 이 질문에 대한 답은 원시 어레이을 사용할 때 강조 표시됩니다.

   
1. 기본 어레이의 경우, `Arrays.stream()` 그리고 `Stream.of()` 다른 반환 유형이 있습니다. 예를 들어 기본 정수 어레이을 전달하면 `Stream.of()` 메서드 반환 `Stream<int[]>`, 반면 `Arrays.stream()` 반환 `IntStream`.

|   |   |
|---|---|
|1<br><br>2<br><br>3|int[] array = { 1, 2, 3, 4, 5 };<br><br>Arrays.stream(array)                    // IntStream 반환<br><br>    .forEach(System.out::println);      // 인쇄물 1, 2, 3, …|

|   |   |
|---|---|
|1<br><br>2<br><br>3|int[] array = { 1, 2, 3, 4, 5 };<br><br>Stream.of(array)                        // returns `Stream<int[]>`<br><br>    .forEach(System.out::println);      // 인쇄물 [I@27d6c5e0|

   
2. 이후 `Stream.of()` 보고 `Stream<int[]>` 정수 어레이을 사용하면 명시적으로 다음으로 변환해야 합니다. `IntStream` 섭취하기 전에 아래와 같이

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4|int[] array = { 1, 2, 3, 4, 5 };<br><br>Stream.of(array)                        // returns `Stream<int[]>`<br><br>    .flatMapToInt(Arrays::stream)       // IntStream 반환<br><br>    .forEach(System.out::println);|

   
3. `Arrays.stream()` 메서드는 기본 어레이에 대해 오버로드됩니다. `int`, `long`, 그리고 `double` 유형. 다른 기본 유형의 경우 `Arrays.stream()` 작동하지 않습니다. 그것은 반환 `IntStream` 위해 `int[]` 어레이, `LongStream` 위해 `long[]` 어레이과 `DoubleStream` 위해 `double[]` 어레이. 반면에, `Stream.of()` 기본 어레이에 대한 오버로드된 메서드가 없습니다. 대신 개체를 사용할 수 있는 다음과 같은 오버로드된 버전이 있습니다.

|   |   |
|---|---|
|1<br><br>2<br><br>3<br><br>4<br><br>5<br><br>6<br><br>7<br><br>8<br><br>9<br><br>10|/**<br><br>* 단일 요소를 포함하는 순차적인 {@code Stream}을 반환합니다.<br><br>*<br><br>* @param `t` 단일 요소<br><br>* @param `<T>` 스트림 요소의 유형<br><br>* @싱글톤 순차 스트림 반환<br><br>*/<br><br>public static<T> Stream<T> of(T t) {<br><br>    return StreamSupport.stream(new Streams.StreamBuilderImpl<>(t), false);<br><br>}|

   
어레이은 Java의 객체이기도 하므로 기본 어레이을 다음으로 전달할 때 위의 메서드가 호출됩니다. `Stream.of()`.

그것이 바로 차이점에 관한 것입니다. `Stream.of()` 그리고 `Arrays.stream()` Java의 메소드.