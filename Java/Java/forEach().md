- forEach() [[메서드(Method)]]는 개체 컬렉션을 반복하고 각 항목에 대한 작업을 수행하는 데 사용할 수 있다.


- 밑의 코드 예시와 같다.

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
  public static void main(String[] args) {
    List<String> list = new ArrayList<>();
    list.add("one");
    list.add("two");
    list.add("three");
    
    Iterable<String> iterable = list; // 업캐스팅
    
    iterable.forEach((item) -> {
      System.out.println(item);
    });
  }
}
```

- 위 코드에서 문자열 목록을 만들고 여기에 세 개의 항목을 추가한다. 
- 그런 다음 목록을 사용하여 [[Iterable]] [[객체(Object)]]를 만들고 forEach() 메서드와 [[람다식(lambda)]] 사용하여 각 항목을 출력한다.
