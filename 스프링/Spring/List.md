
- List는 [[인터페이스(Interface)]]이다.
- 즉, Java의 다형성에 의해서 만약 아래와 같이 list를 List 자료형으로 선언한 경우, 그 구현체를 ArrayList로도 구현할 수 있지만 LinkedList로 구현 할수도 있다.

```java
List<Integer> list = new ArrayList<Integer>();
list = new LinkedList<Integer>(); // 요구사항 변경 등의 이유로 구현체가 바뀌어도 호환 가능
```

## ArrayList

ArrayList는 클래스입니다. ArrayList 역시 아래처럼 List 인터페이스를 구현하고 있기 때문에 List가 제공하는 기능들을 다 제공할 수 있으므로 List 선언 시 구현 인스턴스의 자료형으로 작성할 수 있습니다. 하지만, List와 다르게 ArrayList의 자료형으로 생성한 경우 LinkedList로 새롭게 구현체를 만들거나 형변환을 할 수는 없습니다.
List<객체> findAll()

이 [[메서드(Method)]]는 모든 Member 객체들을 리스트 형태로 반환한다.