- 자바에서 [[Set]] 또는 [[HashSet]], [[List]] 클래스는 addAll() [[메서드(Method)]] 제공한다.

- 이 메서드는 인자로 전달된 [[Collection]] 객체의 모든 요소를 [[Set]]에 추가한다.


## 문법

- HashSet.addAll()의 문법는 아래와 같다.
- 인자로 List 또는 Set와 같은 Collection 객체를 받으며, 이 객체의 모든 요소들을 Set에 추가한다.

```java
boolean addAll(Collection<? extends E> var1);
```

## 예시

```java
import java.util.*;

public class Example {
    public static void main(String[] args) {
		
        Set<String> set1 = new HashSet<>(Arrays.asList("apple", "grape", "banana", "kiwi"));
        System.out.println("set1: " + set1);
		
        Set<String> set2 = new HashSet<>();
        set2.add("melon");
        set2.addAll(set1);
        System.out.println("set2: " + set2);
    }
}

// >> set1: [banana, apple, kiwi, grape]
// >> set2: [banana, apple, kiwi, grape, melon]
```

- HashSet뿐만 아니라, [[List]]도 [[Collection]]이기 때문에, 아래와 같이 HashSet.addAll()의 인자로 전달하여 모든 요소를 추가할 수 있다.

```java
import java.util.*;

public class Example1 {
    public static void main(String[] args) {
		
        List<String> list = new ArrayList<>(Arrays.asList("apple", "grape", "banana", "kiwi"));
        System.out.println("list: " + list);
		
        Set<String> set = new HashSet<>();
        set.addAll(list);
        System.out.println("set: " + set);
    }
}

// >> list: [apple, grape, banana, kiwi]
// >> set: [banana, apple, kiwi, grape]
```
