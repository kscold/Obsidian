- 로직을 작성하다 보면 [[ArrayList]]를 [[배열(Array)]]로 변환해야 할 때가 있는데, 그 때 사용하는 [[메서드(Method)]]이다.

## 예시

- [[배열(Array)]]을 생성하여 numbers에 담긴 요소(element)를 담고 출력하는 예시이다.

```java
ArrayList<Integer> numbers = new ArrayList<>();

numbers.add(10);
numbers.add(20);
numbers.add(30);
numbers.add(40);
numbers.add(50);

Integer[] array = numbers.toArray(new Integer[numbers.size()]);

for (int i = 0; i < array.length; i++) {
	System.out.println(array[i]);    
}

// >> 10 20 30 40 50
```

  - [[배열(Array)]]로 옮겨진 요소(element)가 출력되는 것을 볼 수 있다.