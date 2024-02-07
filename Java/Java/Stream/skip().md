- [[스트림(Stream)]]의 처음 n개의 요소를 버리는 작업이다.

- [[스트림(Stream)]]의 첫 번째 요소를 버린 후 이 스트림의 나머지 요소로 구성된 스트림을 반환한다.
- 이 스트림에 요소보다 적은 수 n의 요소가 포함되어 있으면 빈 스트림이 반환된다.

- 이것은 상태 저장 중간 작업(stateful intermediate operation)이다.

## 예시

```java
Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    .filter(i -> i % 2 == 0) // [2, 4, 6, 8, 10]
    .skip(2) // [6, 8, 10]
    .forEach(i -> System.out.print(i + " ")); //output: 6 8 10
```

- Stream의 짝수 번호를 필터링한후, 처음 2개를 버리고 그 다음 [[forEach()]]문에서 결과를 출력한다.