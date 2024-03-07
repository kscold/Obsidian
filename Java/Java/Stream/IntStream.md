

- Java8에서 데이터를 더 효과적으로 처리할 수 있는 새로운 기능인 [[스트림(Stream)]] API 중 한 부분이다.
- 'int' 에 대한 순차 및 병렬 집계 연산을 수행하는 데 사용한다.
- 반복문 없이도 [[배열(Array)]]이나 컬렉션([[Collection]])의 데이터를 처리할 수 있다.

## [[메서드(Method)]]

### range()와 rangeClosed()

- 'range' 와 'rangeClosed' 메서드는 주어진 범위 내 순차적인 정수 스트림을 반환한다.
- 'range'는 마지막 정수를 포함하지 않는 반면, 'rageClosed'는 마지막 정수를 포함한다.

```java
IntStream.range(1, 5).forEach(System.out::print); // 출력: 1234
IntStream.rangeClosed(1, 5).forEach(System.out::print); // 출력: 12345
```

### of()

- 지정된 값을 포함하는 순차적인 정수 스트림을 반환한다.
- [[Stream.of()]]와 동작이 비슷하다.

```
IntStream.of(1, 2, 3, 4, 5).forEach(System.out::print); // 출력: 12345
```
### map

- 주어진 함수를 스트림의 **각 요소에 적용한 결과로 구성**된 스트림을 반환한다.
### filter

- 주어진 술어(predicate)와 **일치하는 요소만 포함**하는 스트림을 반환한다.

```java
IntStream.rangeClosed(1, 5)
    .map(n -> n * n)
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println); // 출력: 4, 16
```

### sum

- 각각 스트림의 요소의 합계 반환한다.

### average

- 각각 스트림의 요소의 평균을 반환한다.

### max

- 각각 스트림의 요소의 최대값 반환한다.

### min

- 각각 스트림의 요소의 최소값 반환한다.

```java
int sum = IntStream.rangeClosed(1, 5).sum();
double avg = IntStream.rangeClosed(1, 5).average().getAsDouble();
int max = IntStream.rangeClosed(1, 5).max().getAsInt();
```

### anyMatch 

- [[스트림(Stream)]]의 요소 중 어느 하나라도 주어진 술어를 만족하면 true 를 반환한다.

```java
boolean anyMatch = IntStream.of(1, 2, 3, 4, 5).anyMatch(n -> n == 3);
System.out.println(anyMatch); // 출력: true
```
