- static 키워드는 특정 멤버 혹은 [[Java/필드(Field)|필드(Field)]]([[변수(Variable)]], [[메서드(Method)]], 블록 등)가 [[클래스(Class)]] 속해 있음을 나타낸다. 
- 즉, [[정적 메서드(Static Method)]]를 선언하거나 [[클래스 변수(Class Variable)]]를 선언할 때 static 키워드를 사용한다.

- static으로 선언된 멤버는 클래스의 [[인스턴스(Instance)]]에 속하는 것이 아니라 [[클래스(Class)]] 자체에 속하게 된다.
- static을 붙이면 메모리에 딱 한 번만 할당되어 메모리를 효율적으로 사용할 수 있다.
- 즉, [[Java/필드(Field)]]가 된다.

## 클래스 로딩 시점에 초기화

- Java에서 static 변수는 해당 클래스가 JVM(Java Virtual Machine)에 로딩될 때 한 번 초기화된다. 

- 그 이후로 추가적인 [[객체(Object)]] 생성 없이도 접근 가능클래스 레벨 공유: [[static]] 변수는 모든 [[인스턴스(Instance)]]들 사이에서 공유되므로 하나의 값을 모든 객체가 공유하게 된다. 
- 따라서 한 인스턴스에서 해당 변수를 변경하면, 그 변경사항은 다른 모든 인스턴스에서도 보여지게 된다.

- [[인스턴스(Instance)]] 생성 없이 접근 가능하다.
- static으로 선언된 멤버들은 객체를 생성하지 않고도 사용할 수 있다.
- 즉, [[클래스(Class)]] 이름만으로 접근할 수 있다.

## 예시

- 아래는 static을 활용을 볼 수 있는 예시 코드이다.

```java
public class StaticInstanceTest {
    public static void main(String[] args) { // psvm으로 바로 생성 가능
        StaticInstance object1 = new StaticInstance(); // 객체 인스턴스화
        StaticInstance object2 = new StaticInstance();

        // 객체의 인스턴스 변수 접근
        object1.number1 = 10; // 독립된 인스턴스1
        object2.number1 = 50; // 독립된 인스턴스2
        System.out.println(object1.number1); // 10 출력
        System.out.println(object2.number1); // 50 출력

        // 객체의 클래스 변수 접근
        object1.number2 = 10; // 둘 다 독립된 인스턴스이나 number2가 static필드 이므로 공유
        object2.number2 = 50;
        System.out.println(object1.number2); // 50 출력
        System.out.println(object2.number2); // 50 출력
    }
}

class StaticInstance {
	int number1 = 0; // 인스턴스 변수
	int static number2 = 0; // 클래스 변수
}
```


## static와 static final의 차이

### static

- static 키워드를 가진 멤버는 값이 클래스의 모든 인스턴스에 대해 동일하다.  
- 클래스의 모든 인스턴스가 액세스할 수 있는 **전역 변수**라고 볼 수 있다.
- 그러나 static 변수는 상수가 아니므로 언제나 변경될 수 있다.
### static [[final]]

- 변경할 수 없고 클래스의 모든 인스턴스에 대해 동일한 값을 설정할 때는 static final을 사용해야 한다.
- static 키워드는 값이 클래스의 모든 인스턴스에 대해 동일함을 의미한다.
- final 키워드는 변수에 값이 할당되면 절대 변경할 수 없음을 의미한다.
- 자바에서 static final의 조합은 클래스 변수의 상수 값을 만드는 방법이다.