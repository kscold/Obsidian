- List는 [[인터페이스(Interface)]]이고 ArrayList는 [[클래스(Class)]]이다.
- ArrayList는 자바에서 기본적으로 많이 사용되는 [[클래스(Class)]]이다.
- ArrayList는 자바의 List [[인터페이스(Interface)]]를 상속받은 여러 클래스 중 하나이다.
- 일반 [[배열(Array)]]과 동일하게 연속된 메모리 공간을 사용하며 인덱스는 0부터 시작한다.

![](https://blog.kakaocdn.net/dn/dTm8ja/btqMYaopkFc/uZL2zJbY7fFvX0NrF0Sudk/img.png)

- [[배열(Array)]]과의 차이점은 배열이 크기가 고정인 반면 ArrayList는 크기가 가변적으로 변한다.
- 내부적으로 저장이 가능한 메모리 용량(Capacity)이 있으며 현재 사용 중인 공간의 크기(Size)가 있다.
- 만약 현재 가용량(Capacity) 이상을 저장하려고 할 때 더 큰 공간의 메모리를 새롭게 할당한다.
- 데이터 중복이 가능하며, null 값도 허용한다.

- 자료를 대량으로 추가하거나 삭하면 내부 처리 작업이 늘어나서 성능이 떨어 질 수 있다.

### 1. ArrayList 생성
- 자바에서 ArrayList를 사용하려면 아래 구문을 먼저 추가해야 한다.

```java
import java.util.ArrayList;
```

- ArrayList의 생성은 다음과 같은 구문들로 가능하다.

```java
ArrayList<Integer> integers1 = new ArrayList<Integer>(); // 타입 지정
ArrayList<Integer> integers2 = new ArrayList<>(); // 타입 생략 가능
ArrayList<Integer> integers3 = new ArrayList<>(10); // 초기 용량(Capacity) 설정
ArrayList<Integer> integers4 = new ArrayList<>(integers1); // 다른 Collection값으로 초기화
ArrayList<Integer> integers5 = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5)); // Arrays.asList()
```

- 보통 생성할 때는 new ArrayList<>()와 같이 타입을 생략해서 작성한다.
- ArrayList를 생성할 때 [[Java/Set]]이나 다른 ArrayList를 전달하면 해당 [[컬렉션(Collection)]]의 값들로 초기화된다.
- 마지막으로 가변 인자를 전달받는 Arrays.[[asList()]]를 사용하면 기본 값들로 생성 가능하다.

### 2. ArrayList 엘레멘트 추가/변경
- ArrayList를 생성한 후 [[add()]] 메서드로 엘레멘트를 추가할 수 있다.
- 또한 set() 메서드로 기존에 추가된 값을 변경하는 것도 가능하다.

```java
import java.util.ArrayList;

public class ArrayListTest {
    public static void main(String[] args) {
        ArrayList<String> colors = new ArrayList<>();
        // add() method
        colors.add("Black");
        colors.add("White");
        colors.add(0, "Green");
        colors.add("Red");

        // set() method
        colors.set(0, "Blue");

        System.out.println(colors);
    }
}
```

- Black과 White는 순서대로 추가가 되며 Green이 첫 번째에 추가되면서 Black과 White는 각각 한 칸씩 밀리게 된다.
- 그리고 Red가 맨 끝에 다시 추가되는 것을 확인할 수 있다.
- 마지막으로 set() 메소드를 통해 가장 앞(Index: 0)의 Green이 Blue로 변경된다.

- 따라서 결과는 아래와 같이 출력된다.
![](https://blog.kakaocdn.net/dn/cjTrLd/btqM4z1RyZB/aGIkXjHSxyf11QhDu7TjvK/img.png)

#### **3. ArrayList 엘레멘트 삭제**

- 추가했던 값을 삭제할 때는 remove() 메소드를 호출한다.

```java
import java.util.ArrayList;
import java.util.Arrays;

public class ArrayListTest {
    public static void main(String[] args) {
        ArrayList<String> colors = new ArrayList<>(Arrays.asList("Black", "White", "Green", "Red"));
        String removedColor = colors.remove(0);
        System.out.println("Removed color is " + removedColor);

        colors.remove("White");
        System.out.println(colors);

        colors.clear();
        System.out.println(colors);
    }
}
```

- 위의 코드를 보면 일반 배열을 ArrayList로 바꾸기 위해 [[asList()]] 메서드를 사용했다.
- 삭제할 때는 엘레멘트의 인덱스를 입력하거나 엘레멘트를 직접 입력할 수 있다.
- 인덱스를 통해 삭제할 경우 삭제되는 엘레멘트를 리턴받을 수 있다.
- 값을 지움과 동시에 해당 값으로 별도의 작업이 필요한 경우 리턴을 받아서 사용하면 된다.
- ArrayList 안의 내용을 전체 삭제할 때는 clear()를 호출하면 된다.

#### **4. ArrayList 전체 값 확인**
- ArrayList의 모든 값들을 순회해서 출력하고 싶은 경우 다양한 방법을 사용할 수 있다.

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Iterator;
import java.util.ListIterator;

public class ArrayListTest {
    public static void main(String[] args) {
        ArrayList<String> colors = new ArrayList<>(Arrays.asList("Black", "White", "Green", "Red"));
        // for-each loop
        for (String color : colors) {
            System.out.print(color + "  ");
        }
        System.out.println();

        // for loop
        for (int i = 0; i < colors.size(); ++i) {
            System.out.print(colors.get(i) + "  ");
        }
        System.out.println();

        // using iterator
        Iterator<String> iterator = colors.iterator();
        while (iterator.hasNext()) {
            System.out.print(iterator.next() + "  ");
        }
        System.out.println();

        // using listIterator
        ListIterator<String> listIterator = colors.listIterator(colors.size());
        while (listIterator.hasPrevious()) {
            System.out.print(listIterator.previous() + "  ");
        }
        System.out.println();
    }
}
```

- for-each 반복문으로 각각의 값을 순회해서 출력하는 것이 가능하다.
- 또한 get() 메소드로 각 인덱스의 값을 순차적으로 탐색하는 방법도 가능하다.
- 그리고 iterator나 listIterator를 통해 값들을 순회하는 것도 가능하다.
- listIterator의 경우 생성 시 ArrayList의 크기를 입력해주고 역방향으로 출력할 수 있다.

![](https://blog.kakaocdn.net/dn/DZXoS/btqM9p7K6js/lECX8GgwgS5i1o511AsxAK/img.png)

출력 결과

마지막의 경우 역순서로 출력이 되는 것을 확인할 수 있습니다.

#### **5. 값 존재 유무 확인**

ArrayList의 안에 값이 존재하는지 존재한다면 어느 위치에 존재하는지 알고 싶은 경우가 있습니다.

먼저 값이 존재하는지만 알고 싶은 경우 contains()를 사용합니다.

값이 존재할 때 어느 위치에 존재하는지 알고 싶은 경우 indexOf()를 사용할 수 있습니다.

```
import java.util.ArrayList;
import java.util.Arrays;

public class ArrayListTest {
    public static void main(String[] args) {
        ArrayList<String> colors = new ArrayList<>(Arrays.asList("Black", "White", "Green", "Red"));
        boolean contains = colors.contains("Black");
        System.out.println(contains);

        int index = colors.indexOf("Blue");
        System.out.println(index);

        index = colors.indexOf("Red");
        System.out.println(index);
    }
}
```

contains()는 값이 있는 경우 true를, 값이 없는 경우 false를 리턴합니다.

indexOf()는 값이 존재하는 경우 해당 엘레멘트의 인덱스를 리턴합니다.

값이 존재하지 않을 경우 -1을 리턴하기 때문에 별도로 처리가 가능합니다.



## 이 밖에 주요 메소드
- **.add((index), val)**: 순서대로 리스트를 추가, 배열 사이즈 초과 시 초기 설정된 사이즈만큼 자동으로 사이즈가 증가함, 인덱스를 추가로 지정해주면 해당 인덱스에 값을 삽입
- **.get(index)**: 해당 인덱스의 값 반환
- **.set(index, val)**: 인덱스로 값 변경
- **.indexOf(val)**: 값을 제공하면 해당 값의 첫번째 인덱스를 반환
- **.lastindexOf(val)**: 해당 값의 마지막 인덱스 반환
- **.remove(index or val)**: 해당 인덱스의 값 or 해당 값 중 첫번째 값 삭제
- **.contains(val)**: 해당 값이 배열에 있는지 검색해서 true / false 반환
- .containsAll(val1, val2...): argument로 제공한 컬렉션의 모든 값이 포함되어 있는지 여부를 true / false로 반환
- .toArray(): ArrayList 타입의 인스턴스를 일반 배열 타입으로 반환, 저장할 배열 타입에 맞춰 자동 형변환, 배열 크기 또한 자동으로 맞춰서 바꿔줌
- **.clear()**: 값 모두 삭제
- **.isEmpty()**: 비었으면 true, 하나라도 값이 있으면 false 반환
- .addAll(arr2): 두 컬렉션을 합침
- .retainAll(arr2): argument로 제공한 컬렉션 내에 들어있는 값을 제외하고 모두 지워줌
- **.removeAll(arr2)**: argument로 제공한 컬렉션 내에 들어있는 값과 일치하는 값을 모두 지워줌, retainAll() 메소드와 반대
- **.size()**: 요소 개수 반환

```java
import java.util.ArrayList;

public class arrList {
    public static void main(String[] args){
        ArrayList<String> al = new ArrayList<String>();

        // 리스트 요소 추가
        al.add("박");
        al.add("김");
        al.add("최");
        System.out.println(al); // [박, 김, 최]

        al.add(1, "이");
        System.out.println(al); // [박, 이, 김, 최]

        // 해당 인덱스의 값 반환
        System.out.println(al.get(0)); // 박

        // 인덱스로 값 변경
        al.set(3, "정");
        System.out.println(al); // [박, 이, 김, 정]

        // 인덱스로 값 찾기
        al.add("박"); // [박, 이, 김, 정, 박]
        System.out.println(al.indexOf("박")); // 0
        System.out.println(al.lastIndexOf("박")); // 4

        // 값 삭제
        al.remove(4);
        System.out.println(al); // [박, 이, 김, 정]

        al.add("정"); // [박, 이, 김, 정, 정]
        al.remove("정");
        System.out.println(al); // [박, 이, 김, 정]

        // 값 포함하는지
        System.out.println(al.contains("정")); // true

        // Array로 변환
        System.out.println(al.toArray()); // [Ljava.lang.Object;@7d6f77cc

        // 배열 비우기
        al.clear();
        System.out.println(al); // []

        // 배열 비었는 지 확인
        System.out.println(al.isEmpty()); // true

        // 두 리스트 합치기
        ArrayList<String> al1 = new ArrayList<String>();
        al1.add("박");
        al1.add("김");
        al1.add("이");

        ArrayList<String> al2 = new ArrayList<String>();
        al2.add("최");
        al2.add("정");
        al2.add("박");

        al1.addAll(al2);
        System.out.println(al1); // [박, 김, 이, 최, 정, 박]

        // 한 리스트가 한 리스트 포함하는지
        System.out.println(al1.containsAll(al2)); // true

        // 리스트 해당 요소 빼고 지우기
        al1.retainAll(al2);
        System.out.println(al1); // [박, 최, 정, 박]

        // 리스트 해당 요소 지우기
        al1.removeAll(al2);
        System.out.println(al1); // []

        // 요소 개수 반환
        System.out.println(al1.size()); // 0
        System.out.println(al2.size()); // 3
    }
}
```