- 자바에서 Iterable은 for-each 루프 또는 [[Iterator()]]를 사용하여 반복하거나 반복할 수 있는 [[객체(Object)]] [[Collection]]을 나타내는 [[인터페이스(Interface)]]이다.

- Iterable 인터페이스는 [[Iterator()]] [[객체(Object)]]를 반환하는 단일 [[메서드(Method)]]인 iterator()를 제공한다.
- [[Iterator()]]는 Iterable의 객체 컬렉션을 반복하는 데 사용할 수 있다.

- 따라서 전반적으로 Iterable [[인터페이스(Interface)]]는 for-each 루프 또는 Iterator를 사용하여 반복될 수 있는 자바의 객체 컬렉션을 나타내는 [[메서드(Method)]] 제공한다.

## 문법

- 아래와 같은 [[메서드(Method)]]가 정의되어 있다.
- 마찬가지로 [[Collection]]의 [[List]], [[Set]] 에서도 그냥 이 메서드가 정의만 되어 있다.

```java
/**
* 
@return an Iterator.
*/
Iterator<T> iterator();
```

- 즉, 실제 [[iterator()]] [[메서드(Method)]]의 구현은 [[자식 클래스(sub class)]]([[ArrayList]], [[LinkedList]] 등)에서 구현을 하고 있다.

- Iterable [[인터페이스(Interface)]]에는 Iterator 구현체를 반환하는 iterator() 메서드가 선언되어 있다.
- 따라서 해당 인터페이스를 상속받은 하위 클래스들은 [[iterator()]] 메서드를 구현해야 하고, 해당 메서드의 구현을 통해 iterator를 사용할 수 있다.

- 또한 이 인터페이스는 for-each loop를 사용할 수 있는 [[클래스(Class)]]라는 것을 명시해주는 기능도 제공한다.
- Iterable을 상속받는 클래스는 Iterator를 사용하여 for-each loop를 사용할 수 있게 된다.

### for-each-loop의 예시

```java
//일반적인 for문
for (int i=0; i<length; i++) {
    …
}

//for each문(향상된 for문)
for (String str : list) {
    System.out.println(str);
}

//for each문이 실제 동작되는 코드(iterator가 사용됨)
Iterator<String> iterator = list.iterator();

while(iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

- 향상된 [[for]]문(The enhanced for statement)이라고 불리는 for-each loop는 일반 for loop처럼 for 키워드로 시작한다.

- 루프 카운터 변수를 선언하고 초기화하는 대신, [[배열(Array)]]의 기본 유형과 동일한 유형인 변수를 선언하고 그 뒤에 콜론과 루프를 돌릴 [[배열(Array)]]을 차례로 선언한다.
- 루프 본문에서는 인덱스 배열 요소를 사용하는 대신 생성된 루프 변수를 사용할 수 있게 된다.

- [[배열(Array)]] 또는 [[Collection]] [[클래스(Class)]]를 반복하는데만 사용할 수 있다.