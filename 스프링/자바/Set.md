- 자바에서 Set은 중복을 허용하지 않고 순서가 없는 데이터 구조이다.
- Set은 집합 개념을 기반으로 하며, 고유한 값을 유지하고 검색, 추가, 제거 등의 작업을 효율적으로 수행할 수 있다.


![[Pasted image 20240104211842.png]]
## [[HashSet]] [[클래스(Class)]]

- HashSet은 자바에서 가장 일반적으로 사용되는 Set 인터페이스의 구현 클래스이다.
- HashSet은 해시 테이블을 사용하여 요소를 저장하며, 순서가 보장되지 않는다.
- HashSet은 다양한 데이터 유형을 저장할 수 있으며, 중복된 값을 자동으로 제거한다.

```java
Set<String> names = new HashSet<>();
names.add("Alice");
names.add("Bob");
names.add("Alice"); // 중복된 값이므로 무시됨

System.out.println(names); // 출력: [Alice, Bob]
```

위의 예시에서는 HashSet을 사용하여 문자열을 저장하고 있습니다. "Alice"는 중복된 값이므로 한 번만 저장되고, 순서는 보장되지 않으므로 출력 결과가 다를 수 있습니다.

## TreeSet 클래스

TreeSet은 Set 인터페이스의 다른 구현 클래스로, 이진 검색 트리(Binary Search Tree)를 사용하여 요소를 저장합니다. TreeSet은 요소들을 정렬된 상태로 유지하며, 자동으로 중복된 값을 제거합니다.

```
Set<Integer> numbers = new TreeSet<>();
numbers.add(5);
numbers.add(2);
numbers.add(8);
numbers.add(2); // 중복된 값이므로 무시됨

System.out.println(numbers); // 출력: [2, 5, 8]
```

위의 예시에서는 TreeSet을 사용하여 정수를 저장하고 있습니다. TreeSet은 요소를 자동으로 정렬하므로 출력 결과는 오름차순으로 나타납니다.

## Set의 주요 메서드

Set 인터페이스는 여러 유용한 메서드를 제공합니다. 몇 가지 주요 메서드는 다음과 같습니다.

- add(element): Set에 요소를 추가합니다.
- remove(element): Set에서 요소를 제거합니다.
- contains(element): Set에 특정 요소가 있는지 확인합니다.
- size(): Set의 요소 개수를 반환합니다.

```
Set<String> colors = new HashSet<>();
colors.add("Red");
colors.add("Green");
colors.add("Blue");

System.out.println(colors.size()); // 출력: 3
System.out.println(colors.contains("Green")); // 출력: true

colors.remove("Red");
System.out.println(colors); // 출력: [Green, Blue]
```

위의 예시에서는 Set에 대한 몇 가지 기본적인 메서드를 사용하여 요소를 추가, 제거, 확인하고 있습니다.

## 결론

자바의 Set에 대해 알아보았습니다. Set은 중복을 허용하지 않고 순서가 없는 데이터 구조로, HashSet과 TreeSet을 통해 구현할 수 있습니다. Set은 고유한 값을 유지하고 검색, 추가, 제거 등의 작업을 효율적으로 처리하는데 유용합니다. 이를 통해 데이터 집합을 다룰 때 Set을 활용할 수 있으며, 다양한 애플리케이션에서 유용하게 활용될 수 있습니다.