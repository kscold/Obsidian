- @Component [[어노테이션(Annotation)]]의 경우 개발자가 직접 작성한 [[클래스(Class)]]를 [[Bean]]으로 등록하기 위한 [[어노테이션(Annotation)]]이다. 


## 예시
```java
@Component
public class Student {
	
    public Student() {
	    System.out.println("hi");
    }
}
```

- Student Class는 개발자가 사용하기 위해서 직접 작성한 Class이다 이러한 클래스를 Bean으로 등록하기 위해 상단에 @Component어노테이션을 사용한것을 볼 수 있다.

```java
@Component(value="mystudent")
public class Student {

    public Student() {
        System.out.println("hi");
    }
}
```

- @Component 역시 아무런 추가 정보가 없다면 Class의 이름을 Camelcase로 변경한 것이 Bean id로 사용된다.
- 하지만 [[@Bean]]과 다르게 @Component는 name이 아닌 value를 이용해 Bean의 이름을 지정한다.