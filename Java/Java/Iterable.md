- Java에서 Iterable은 for-each 루프 또는 [[Iterator]]를 사용하여 반복하거나 반복할 수 있는 개체 컬렉션을 나타내는 [[인터페이스(Interface)]]이다.

- Iterable 인터페이스는 Iterator [[객체(Object)]]를 반환하는 단일 [[메서드(Method)]]인 iterator()를 제공한다.
- Iterator 객체는 Iterable의 객체 컬렉션을 반복하는 데 사용할 수 있다.

- 밑의 코드는 Iterable을 사용하는 방법의 예시이다.

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class Main {
  public static void main(String[] args) {
	List<String> list = new ArrayList<>(); // ArrayList를 생성
    list.add("one"); // List에 값을 추가
    list.add("two");
    list.add("three");
    
    Iterable<String> iterable = list; // 업캐스팅
    
    Iterator<String> iterator = iterable.iterator(); // 반복기로 설정
    
    while (iterator.hasNext()) { // 반복이 true이면
      String item = iterator.next(); // iterator.next는 요소를 item에 대입
      System.out.println(item); // item 출력
    }
  }
}
```

- 위의 코드에서는 문자열 목록을 만들고 여기에 세 개의 항목을 추가한다.
- 그런 다음 목록을 사용하여 Iterable 객체를 만들고 Iterable에서 Iterator를 가져온다. 
- 마지막으로 while 루프를 사용([[hasNext()]]가 True이면)하여 컬렉션을 반복하고 각 항목을 출력(iterator.[[next()]] 대입)한다.

Iterator 메서드 외에도 Iterable 인터페이스는 [[forEach()]]라는 기본 메서드도 제공한다. 
전반적으로 Iterable 인터페이스는 for-each 루프 또는 Iterator를 사용하여 반복될 수 있는 Java의 객체 컬렉션을 나타내는 방법을 제공합니다. 이는 데이터 콜렉션을 사용하여 작업할 수 있는 유연한 방법을 제공하며 Java 에코시스템 전체에서 널리 사용됩니다.