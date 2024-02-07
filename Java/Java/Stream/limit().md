- 이 스트림의 요소로 구성된 스트림 maxSize을 길이보다 길지 않게 잘린 [[스트림(Stream)]]을 반환한다.
- 이것은 단락 상태 저장 중간 작업(short-circuiting stateful intermediate operation) 이다.

## 예시

```java
Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    .filter(i -> i % 2 == 0) // [2, 4, 6, 8, 10]
    .limit(2) // [2, 4]
    .forEach(i -> System.out.print(i + " ")); //output: 2 4
```

- 전체 스트림을 소비하는 [[skip()]]과 달리 limit()는 최대 항목 수에 도달하는 즉시 더 이상 element를 소비하지 않고 결과 스트림을 반환한다.