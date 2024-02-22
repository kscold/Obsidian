- 자바 언어에서 final은 오직 한 번만 할당할 수 있는 [[엔티티(Entity)]]를 정의할 때 사용된다.
- final로 선언된 변수가 할당되면 항상 같은 값을 가진다.

- 만약 final [[변수(Variable)]]가 [[객체(Object)]]를 참조하고 있다면, 그 객체의 상태가 바뀌어도 final 변수는 매번 동일한 내용을 참조한다.

- 즉, 한 번 값을 넣어두면 절대 바뀌지 않는 값이다.

- final은 [[클래스(Class)]], [[메서드(Method)]], [[변수(Variable)]] 각각에 붙을 수 있다.

### final 클래스

```java
public final class MyFinalClass {...}    // final 클래스 선언

public class ThisIsWrong extends MyFinalClass {...} // 상속 불가!
```

 - final이 붙어있는 클래스는 상속할 수 없다.
 - 이렇게 하면 보안이나 효율성 측면에서 장점이 있다. 
 - `java.lang.System`이나 `java.lang.String`처럼 자바에서 기본적으로 제공하는 라이브러리 클래스는 final을 사용한다.

### final 메서드

```java
public class Base
{
    public       void m1() {...}
    public final void m2() {...}    // final 메서드 선언
}

public class Derived extends Base
{
    public void m1() {...}  // Base 클래스의 m1() 상속
    public void m2() {...}  // 오버라이딩, 즉 상속받은 메서드 수정 불가
}
```

- 만약 어떤 클래스를 상속하는데 그 안에 final 메서드가 있다면, [[오버라이딩(Overriding)]]으로 수정할 수 없다.

### final 변수

```java
public class Sphere {

    // PI 변수는 상수로 선언되어 수정할 수 없음
    public static final double PI = 3.141592653589793;

    public final double radius;
    public final double xPos;
    public final double yPos;
    public final double zPos;

    Sphere(double x, double y, double z, double r) {
         radius = r;
         xPos = x;
         yPos = y;
         zPos = z;
    }

    [...]
}
```

- final 변수는 한 번 값을 할당하면 수정할 수 없다.
- 즉, 초기화는 한 번만 가능하다.

```java
public class Test {
  // 선언만 하고 초기화는 각 인스턴스에서 진행
  private final int value;

  public Test(int value) {
	this.value = value;
  }

  public int getValue() {
    return value;
  }
}
```

- 이렇게 먼저 선언해놓고 각각 다른 값을 갖도록 초기화 할 수도 있다.
- 물론 이렇게 한 번 값을 할당하면 다음부터는 수정할 수 없다.
- 이런 형태를 `blank final` 변수라고 한다.

# final 변수의 짝꿍, [[static]]

- final 변수에서 봤던 예시를 다시 한 번 보자.

```java
public class Sphere {

    // PI 변수는 상수로 선언되어 수정할 수 없음
    public static final double PI = 3.141592653589793;

    public final double radius;
    public final double xPos;
    public final double yPos;
    public final double zPos;

    Sphere(double x, double y, double z, double r) {
         radius = r;
         xPos = x;
         yPos = y;
         zPos = z;
    }

    [...]
}
```

- final 변수 앞에 static이 붙어으므로 클래스애 귀속되고 바뀌지 않는 상수 값이 된다.

### static과 final의 궁합

- final 변수를 쓰면 그 값을 계속 그대로 쓴다는 의미이다.
- 어차피 같은 값 쓸 바에 메모리 낭비할 거 없이 하나로 쭉 써도 되기 때문에 이렇게 static과 final을 같이 쓰면 효율성이 높아져서 자주 쓰인다.

- 주의할 점으로 배웠던 blank final 변수(인스턴스 단에서 초기화가 한번 일어나는 변수)는 [[인스턴스(Instance)]]마다 다른 값을 갖는기 때문에 이렇게 final 이어도 초기화가 다르게 된다면 static을 사용하지 않게 된다.
