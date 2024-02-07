- [[ArrayList]]에 들어있는 elemet(요소)를 교체하고 싶을 떄 사용한다.

## 문법

```java
set(int index, E element)
```

## 예시

- 아래 코드는 0 index를 100으로 교체하는 예시이다.

```java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(10);
numbers.add(20);
numbers.add(30);
numbers.add(40);
numbers.add(50);
numbers.set(0, 100);

System.out.println(numbers); 

// >> [100, 20, 30, 40, 50]
```

- 원래 10이었던 값이 100으로 변한 것을 볼 수 있다.