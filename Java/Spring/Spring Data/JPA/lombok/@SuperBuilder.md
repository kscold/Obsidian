- @SuperBuilder는 [[롬복(lombok)]] 라이브러리에서 제공하는 [[어노테이션(Annotation)]] 중 하나이다.
- 이 [[어노테이션(Annotation)]]을 사용하면 자바 빈(Java [[Bean]]) [[클래스(Class)]]를 [[빌더 패턴(Builder Pattern)]]으로 간편하게 구현할 수 있다.

- [[빌더 패턴(Builder Pattern)]]은 [[객체(Object)]]를 생성하기 위한 여러 속성을 가진 [[클래스(Class)]]를 생성하고, 이를 이용하여 객체를 생성하는 방식이다.
- 빌더 패턴은 객체 생성을 보다 유연하고 가독성 높은 방식으로 구현할 수 있어, 객체 생성 로직이 복잡한 경우 유용하다.

- @SuperBuilder 어노테이션은 @Builder 어노테이션의 기능을 보완하기 위해 도입되었다.
- [[@Builder]] [[어노테이션(Annotation)]]으로는 [[상속(Inheritance)]]받은 [[Java/필드(Field)|필드(Field)]]를 빌더에서 사용하지 못하는 등의 제한이 있었다.
- @SuperBuilder [[어노테이션(Annotation)]]은 이러한 제한을 해결하고, 상속받은 필드도 빌더에서 사용할 수 있다.

- @SuperBuilder 어노테이션은 다음과 같은 장점이 있다.
- 빌더 패턴을 구현하기 위한 코드를 간결하게 작성할 수 있다.
- 별도의 빌더 클래스를 작성하지 않아도 된다.
- [[생성자(constructor)]]에서 상속받은 필드도 빌더에서 사용할 수 있다.

## 예시

- SuperBuilder를 사용하기 위해서는 [[부모 클래스(super class)]]와 [[자식 클래스(sub class)]] 양쪽에 @SuperBuilder 어노테이션을 추가해줘야 한다.
- [[자식 클래스(sub class)]]를 생성하면서 빌더를 통해 부모와 자식 클래스의 [[Java/필드(Field)|필드(Field)]] 모두를 작성할 수 있다.

### 부모 클래스 Parent

```java
import lombok.experimental.SuperBuilder;

@SuperBuilder
public class Parent {
    private String parentField;
}
```

### 자식 클래스 Child

```java
import lombok.experimental.SuperBuilder;

@SuperBuilder
public class Child extends Parent {
    private String childField;
}
```

- 아래는 SuperBuilder를 테스트하는 코드이다.

```java
import org.junit.jupiter.api.Test;

class SupperBuilderTest {

    @Test
    void supperBuilderTest() {
        Child child = Child.builder()
                .parentField("parent") // 부모 클래스 생성자
                .childField("child") // 자식 클래스 생성자
                .build(); // 생성
    }

}
```