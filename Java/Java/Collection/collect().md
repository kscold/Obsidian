- [[스트림(Stream)]]의 최종 연산자 중 가장 복잡하지만, 가장 유용하게 활용될수 있는 것이 바로 collect()이다. 
- collect() 또한 reduce()와 유사하다. 

- 하지만 추가적으로 어떻게 수집할 것인가에 대한 방법이 정의되어 있는데, 이를 구현하는 것이 collector 이다.

- collector는 Collector [[인터페이스(Interface)]]를 구현한 것이고, 직접 구현할 수도 있지만, 미리 작성된 것을 사용하기도 한다. 
- Collectors [[클래스(Class)]]는 미리 작성된 다양한 collector를 반환하는 [[static]] [[메서드(Method)]]를 가지고 있다.
- 구현된 collector를 collect() 메서드의 인자로 넣어 사용한다. 

| collect() | [[스트림(Stream)]]의 [[최종 연산]], 매개변수로 컬렉터를 필요로 한다. |
| ---- | ---- |
| Collector | [[인터페이스(Interface)]]로 컬렉터는 이를 구현해야 한다. |
| Collectors | [[클래스(Class)]]로 [[static]] [[메서드(Method)]]로 미리 구현한 컬렉터를 제공한다. |

## 스트림을 [[Collection]]이나 [[배열(Array)]]로 반환하는 방법

### 컬렉션으로 반환 

| [[List]]로 변환  | Collectors.[[toList()]] |
| ---- | ---- |
| [[Map]]으로 변환 | Collectors.toMap() |
| 그 외 Collections로 변환 | Collectors.toCollection([[람다(lambda)]]식) |
- 밑에는 [[Collection]]으로 변환하는 코드 예시이다.

```java
//Collectors.toList()
Stream<String> stream = Stream.of("1", "2", "3", "4", "5"); // 스트림 생성
List<String> streamToList = stream.collect(Collectors.toList()); // 스트림을 리스트로 변환
System.out.println("streamToList = " + streamToList);

//Collectors.toMap()
Stream<String> stream2 = Stream.of("1", "2", "3", "4", "5");
Map<Integer, String> streamToMap = stream2.collect(Collectors.toMap((i) -> Integer.parseInt(i), (i) -> "\""+(i)+"\""));
System.out.println("streamToMap = " + streamToMap);

//List나 Map이 아닌 컬렉션으로의 변환
Stream<String> stream3 = Stream.of("1", "2", "3", "4", "5");
ArrayList<String> streamToArrayList = stream3.collect(Collectors.toCollection(() -> new ArrayList<>()));
System.out.println("streamToArrayList = " + streamToArrayList);
```

### 배열로 반환

- Collect()는 아니지만, [[toArray()]]를 사용한다.

```java
// 배열로 변환
Stream<String> stream4 = Stream.of("1", "2", "3", "4", "5");
Object[] objects = stream4.toArray();

Stream<String> stream5 = Stream.of("1", "2", "3", "4", "5");
String[] stringArray = stream5.toArray((i) -> new String[i]);
```

- 특정한 형을 원할 때에는 인자에 [[람다(lambda)]]식을 넣어주어야 한다.