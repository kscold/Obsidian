- iterator 자체 타입인 반복기(iterator)는 개발자가 컨테이너, 특히 [[List]](리스트)를 순회할 수 있게 해주는 [[객체(Object)]]이다. 
- iterator의 [[인터페이스(Interface)]]와 의미는 고정돼 있지만, iterator는 컨테이너 구현의 기본 구조로 구현되는 경우가 많으며 반복기의 작동 의미를 사용하기 위해 컨테이너와 밀접하게 연결되는 경우가 많다.

- iterator()메서드는 Iterator를 구현한 Itr [[클래스(Class)]]의 [[객체(Object)]]를 반환한다. 
- 따라서 Itr [[클래스(Class)]] 내부에 hasNext(), next() 메서드가 구현된 것도 볼 수 있다.


## iterator() 메서드 (Itr 클래스)

- hasNext(): 다음 요소(element)에 읽어 올 요소가 있는지 확인 하는 메서드로 있으면 true, 없으면 false 를 반환한다.
- next(): 다음 요소(element)를 가져온다. 
- remove(): next()로 읽어온 요소(element)를 삭제한다.

- Iterator 메서드 외에도 java 1.8부터 Iterable 인터페이스는 [[forEach()]]라는 기본 메서드도 제공한다. 

## 예시

- 밑의 코드는 [[Iterable]]의 iterator() 메서드를 [[ArrayList]]에서 구현하여 반복하는 방법의 예시이다.

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class Main {
  public static void main(String[] args) {
	List<String> list = new ArrayList<>(); // ArrayList로 리스트 생성
    
    list.add("one"); // List에 값을 추가
    list.add("two");
    list.add("three");
    
    Iterable<String> iterable = list; // 업 캐스팅
    
    Iterator<String> iterator = iterable.iterator(); // iterator() 메서드로 반복기로 설정
    
    while (iterator.hasNext()) { // 반복할 것이 있으면(true)
      String item = iterator.next(); // iterator.next는 요소를 item에 대입
      System.out.println(item); // item 출력
    }
  }
}
```

- 위의 코드에서는 문자열 목록을 만들고 여기에 세 개의 항목을 추가한다.
- 그런 다음 목록을 사용하여 [[Iterable]] [[객체(Object)]]를 만들고 [[Iterable]]에서 Iterator를 가져온다. 
- 마지막으로 while 루프를 사용([[hasNext()]]가 True이면)하여 [[Collection]]을 반복하고 각 항목을 출력(iterator.[[next()]] 대입)한다.