- implements의 가장 큰 특징은 이렇게 부모 [[메서드(Method)]]를 반드시 [[오버라이딩(Overriding)]](재정의)해야 한다.

```java
interface TestInterface{
  public static int num = 8;
  public void fun1();
  public void fun2();
}

class InterfaceExam implements TestInterface{
  @Override
  public void fun1(){
    System.out.println(num);
  }
  
  @Override
  public void fun2() {
    
  }
}

public class InterfaceSample{
  public static void main(String args[]){
    InterfaceExam exam = new InterfaceExam();
    exam.fun1();
  }
}
```

- 또한 이 implements는 다중상속을 대신해준다.

```java
public class Son implements Father, Mother{...}
```


 -> 그런데 "이러한 구현은 메소드를 어차피 재정의해야되니 '상속'의 의미가 아니지 않냐?" 이런 의문이 들 수 있다. 
 따라서 Java와 C#의 인터페이스는 상속의 개념 보다 계약 및 분류의 의미가 강하다.  
