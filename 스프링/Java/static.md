- static 키워드는 특정 멤버(변수, [[메서드(Method)]], 블록 등)가 [[클래스(Class)]] 속해 있음을 나타낸다. 즉, static으로 선언된 멤버는 클래스의 [[인스턴스(Instance)]]에 속하는 것이 아니라 클래스 자체에 속하게 된다.

- 즉, [[필드(field)]]가 된다.
- 정적 메소드라고도 부른다.

### 클래스 로딩 시점에 초기화
- Java에서 static 변수는 해당 클래스가 JVM(Java Virtual Machine)에 로딩될 때 한 번 초기화된다. 

- 그 이후로 추가적인 [[객체(Object)]] 생성 없이도 접근 가능클래스 레벨 공유: [[static]] 변수는 모든 [[인스턴스(Instance)]]들 사이에서 공유되므로 하나의 값을 모든 객체가 공유하게 된다. 
- 따라서 한 인스턴스에서 해당 변수를 변경하면, 그 변경사항은 다른 모든 인스턴스에서도 보여지게 된다.

- 인스턴스 생성 없이 접근 가능: static으로 선언된 멤버들은 객체를 생성하지 않고도 사용할 수 있다.
- 즉, 클래스 이름만으로 접근할 수 있다.

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