- [[롬복(lombok)]]의 기능으으로 @Builder [[어노테이션(Annotation)]]을 통해 [[빌더 패턴(Builder Pattern)]]을 만든다.

## @Builder Annotation 사용

```java
@Builder
@Getter
public class Test{
    private Integer number;
    private String str;
}

public class TestMain{
   @Test
   void test1(){
       Test test = Test.builder()
                       .number(1)
                       .str("test")
                       .build();
       System.out.println("output : " + test.getNumber() + " " + test.getStr());
   }
}
// >> output : 1 test
```

- 위에서 알 수 있듯이, [[롬복(lombok)]]의 @Builder [[어노테이션(Annotation)]]은 자동으로 Builder Pattern에 맞게 builder 클래스를 생성해주고 그것을 사용할 수 있게 한다.
- 아까 위에서 사용했던 코드보다 코드가 훨씬 줄고, 쉽게 사용할 수 있다는 것이 느껴진다.

## @Builder Annotation을 사용하여 build시 값을 넣지 않는 경우

```java
@Builder
@Getter
public class Test{
    private Integer number;
    private String str;
}

public class TestMain{
   @Test
   void test1(){
       Test test1 = Test.builder()
                       .str("test")
                       .build();
       System.out.println("test1 output : " + test1.getNumber() + " " + test1.getStr());
       Test test2 = Test.builder()
                       .number(1)
                       .build();
       System.out.println("test2 output : " + test2.getNumber() + " " + test2.getStr());
   }
}

// >> test1 output : 0 test
// >> test2 output : 1 null
```

- 위와 같이 builder 를 사용하여 build를 했을 때, 특정 필드에 값을 지정해주지 않으면 0 혹은 null과 같은 값을 보이고 있다.
- 그러면 우리가 이 값을 지정하지 않았을 때, default로 지정하려면 어떻게 해야할까?

### [[Java/필드(Field)|필드(Field)]]에 기본값 초기화(실패 코드)

```java
@Builder
@Getter
public class Test{
    private Integer number = 2;
    private String str = "default";
}

public class TestMain{
   @Test
   void test1(){
       Test test1 = Test.builder()
                       .str("test")
                       .build();
       System.out.println("test1 output : " + test1.getNumber() + " " + test1.getStr());
       Test test2 = Test.builder()
                       .number(1)
                       .build();
       System.out.println("test2 output : " + test2.getNumber() + " " + test2.getStr());
   }
}

// >> test1 output : 0 test
// >> test2 output : 1 null
```

- 의도는 필드에 값을 미리 초기화해두면, 초기화한 값을 쓸 것이라 예상하지만 Builder 패턴을 사용했을 때의 결과를 보면 그렇지 않다는 것을 알 수 있다.

- 이유는 Lombok의 문서에서 Builder에 나와있는데, "builder 사용시 필드에 대한 값을 지정하지 않으면 타입에 따라 0, null, false가 초기화 될테니, @Builder.Default [[어노테이션(Annotation)]]을 명시하고 그 변수에 값을 초기화해라." 이다.

### @Builder.Default 사용

```java
@Builder
@Getter
public class Test{
    @Builder.Default
    private Integer number = 2;
    
    @Builder.Default
    private String str = "default";
}

public class TestMain{
   @Test
   void test1(){
       Test test1 = Test.builder()
                       .str("test")
                       .build();
       System.out.println("test1 output : " + test1.getNumber() + " " + test1.getStr());
       Test test2 = Test.builder()
                       .number(1)
                       .build();
       System.out.println("test2 output : " + test2.getNumber() + " " + test2.getStr());
   }
}

// >> test1 output : 2 test
// >> test2 output : 1 default
```

- 따라서 원했던 결과가 나왔다.