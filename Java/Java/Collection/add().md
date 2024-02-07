- [[배열(Array)]]이나 [[List]], [[ArrayList]]에 요소를 추가하는 [[메서드(Method)]]이다.

- add()는 기본적으로 리스트의 가장 끝에 값을 추가한다.
- 별도로 인덱스를 지정하면 해당 인덱스에 값이 추가되고 그 인덱스부터의 값들이 1 칸씩 밀린다.

- Arrays.[[Arrays.asList()]]로 선언된 [[List]]는 add() 함수 사용이 불가하다.

## 문법

```java
boolean add(Object o) // 성공여부의 boolean을 반환

void add(int index Object element)
```

## 예시

```java
List<String> fruits = new ArrayList<>();
fruits.add("apple");
fruits.add("banana");
fruits.add("cherry");

for (String item : fruits) { // 향상된 for문 사용
	System.out.println(item);
}
 // >> apple
 // >> banana
 // >> cherry
```