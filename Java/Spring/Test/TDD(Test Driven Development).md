- TDD(Test Driven Development) 테스트 주도 개발이라고 부른다.
- 프로덕션 코드보다 테스트 코드를 먼저 작성하는 개발 방법이다.

- TFD(Test First Development) + 리팩토링이다.
- 기능 동작을 검증한다.([[메서드(Method)]] 단위이다.)

## 테스트 코드를 작성하는 이유

1. 문서화 역할을 한다.
2. 코드에 결함을 발견하기 위함이다.
3. 리팩토링 시 안정성을 확보한다.
4. 테스트 하기 쉬운 코드를 작성하다 보면 더 낮은 결합도를 가진 설계를 얻을 수 있다.

- 특히 경계조건에 대해서 테스트 코드를 작성해야 한다.

## [[AssertJ]]

- 테스트 코드 가독성을 높여주는 라이브러리이다.

## [[JUnit5]]

- 자바 단위 테스팅 프레임워크이다.(기본적으로 dependency가 걸려있다.)

## AssertJ을 더 사용해야되는 이유

- 배우기 쉽다.
    - AssertJ Docs 정리가 잘 되어 있다.
- 사용하기 쉽다.
    - 테스트 클래스에 dependency 및 static import만 추가하면 돼서 쓰기 간편하다.
- fluent
    - 유창하다라는 뜻인데, 맞지 않는 것 같아서 그대로 가져왔다.
    - AssertJ는 여러분의 assertions를 다양화하는 데 도움을 준다.
- 코드를 더 읽기 쉽다.
- 자동 완성을 지원한다.
    - 따라서 모든 메서드 이름을 기억할 필요는 없다.

- gradle dependency에 다음 코드를 추가하면 된다.

```java
testImplementation 'org.assertj:assertj-core:3.13.2'
```

- 그 다음 테스트 코드에 static import를 통해 가져와서 사용하면 된다.

```java
import static org.assertj.core.api.Assertions.*;
```

## [[JUnit5]]와 [[AssertJ]]의  차이점

- 다음은 JUnit5를 사용한 코드이다.

```java
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;

public class JUnitTest {
    @Test
    public void testEqual() {
        Object object1 = new Object();
        Object object2 = object1;

        Assertions.assertEquals(object1,object2);
    }
}
```

- 다음은 AssertJ를 사용한 코드이다.

```java
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.*;

public class AssertJTest {
    @Test
    public void testEqual() {
        Object object1 = new Object();
        Object object2 = object1;

        assertThat(object1).isEqualTo(object2);
    }
}
```

- 확실히 JUnit보다는 AssertJ가 더 직관적이다.
- 심지어 AssertJ는 as 메서드를 통해 에러 메시지까지 정의가 가능하다.

```java
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.*;

public class AssertJTest {
    @Test
    public void testEqual() {
        Object object1 = new Object();
        Object object2 = object1;
        Object object3 = new Object();

        // failing assertion, with custom error message
        assertThat(object1).as("Checking whether objects are the same").isEqualTo(object3);
    }
}
```

## Collection Test

- Student라는 Class를 타입으로 가지는 Collection을 테스트하는 코드를 JUnit, AssertJ 둘다 살펴보자.

```java
class Student {
    private String name;
    private int age;
		private int level;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

}
```

### JUnit

- Junit에서는 [[Stream]]과 [[람다(lambda)]]를 사용하여 해결하였다.

```java
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class StudentTest {
    @Test
    public void testStudentArrayList() {
        ArrayList<Student> characters = new ArrayList<>();
        Student frodo = new Student("Frodo", 32);
        Student aragorn = new Student("Aragorn", 62);
        Student sam = new Student("Sam", 36);
        characters.add(frodo);
        characters.add(aragorn);
        characters.add(sam);

        List<Student> expected = new ArrayList<>();
        expected.add(frodo);
        expected.add(aragorn);
        List<Student> filteredList = characters.stream()
                .filter((character) -> character.name.contains("o"))
                .collect(Collectors.toList());

        Assertions.assertEquals(expected,filteredList);

    }
}
```

- 조금은 복잡한 테스트 코드가 됐다.

### AssertJ

- 다음은 AssertJ를 사용한 테스트 코드를 살펴보자.

```java
import org.junit.jupiter.api.Test;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;
import static org.assertj.core.api.Assertions.assertThat;

public class StudentTest {
    @Test
    public void testStudentArrayList() {
        ArrayList<Student> characters = new ArrayList<>();
        Student frodo = new Student("Frodo", 32);
        Student aragorn = new Student("Aragorn", 62);
        Student sam = new Student("Sam", 36);
        
        characters.add(frodo);
        characters.add(aragorn);
        characters.add(sam);
        
        assertThat(characters)
					.filteredOn(character -> character.name.contains("o"))
					.containsOnly(aragorn, frodo);
        
    }
}
```
