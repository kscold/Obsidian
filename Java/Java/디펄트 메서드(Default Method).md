- [[인터페이스(Interface)]]는 기능에 대한 선언만 가능하기 때문에, 실제 코드를 구현한 로직은 포함될 수 없다. 
- 하지만 Java 8에서 부터  Default Method(디펄트 메소드)추가 되었는데 [[메서드(Method)]] 선언 시에 default를 명시하게 되면 인터페이스 내부에서도 로직이 포함된 메서드를 선언할 수 있다.

- 인터페이스의 default method는 'default'라는 키워드를 명시해야 한다.

```java
interface MyInterface { 
    default void printHello() { 
    	System.out.println("Hello World"); 
    } 
}
```

- default라는 키워드를 메서드에 명시하게 되면 인터페이스 내부라도 코드를 작성할 수 있다.

### 왜 사용할까?

사실 인터페이스는 기능에 대한 구현보다는, 기능에 대한 '선언'에 초점을 맞추어서 사용 하는데, 디펄트 메소드는 왜 등장했을까요? 자바 기본서 '자바의 신'에서는 디펄트 메소드에 대한 존재 이유를 아래와 같이 설명했습니다.

> ...(중략) ... 바로 "하위 호환성"때문이다. 예를 들어 설명하자면, 여러분들이 만약 오픈 소스코드를 만들었다고 가정하자. 그 오픈소스가 엄청 유명해져서 전 세계 사람들이 다 사용하고 있는데, 인터페이스에 새로운 메소드를 만들어야 하는 상황이 발생했다. 자칫 잘못하면 내가 만든 오픈소스를 사용한 사람들은 전부 오류가 발생하고 수정을 해야 하는 일이 발생할 수도 있다. 이럴 때 사용하는 것이 바로 default 메소드다. (자바의 신 2권)

기존에 존재하던 인터페이스를 이용하여서 구현된 클래스를 만들고 사용하고 있는데 인터페이스를 보완하는 과정에서 추가적으로 구현해야 할 혹은 필수적으로 존재해야 할 메소드가 있다면, 이미 이 인터페이스를 구현한 클래스와의 호환성이 떨어 지게 됩니다. 이러한 경우 default 메소드를 추가하게 된다면 하위 호환성은 유지되고 인터페이스의 보완을 진행 할 수 있습니다.

실제로 스프링 프레임 워크 버전 4에서 WebMvcConfigure라는 인터페이스를 구현한 클래스 WebMvcConfigurerAdapter를 사용하였지만 자바8에서 default 메소드가 등장하고 WebMvcConfigurerAdapter클래스는 더 이상 사용되지 않습니다. 

### 예제 코드

```java
interface MyInterface { 
    default void printHello() {
    	System.out.println("Hello World"); 
        } 
} 

class MyClass implements MyInterface {} 

public class DefaultMethod { 
    public static void main(String[] args) { 
    	MyClass myClass = new MyClass(); 
        myClass.printHello(); //실행결과 Hello World 출력 
    } 
}
```