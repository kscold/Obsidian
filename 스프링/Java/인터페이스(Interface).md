- 인터페이스(interface)란 다른 클래스를 작성할 때 기본이 되는 틀을 제공하면서, 다른 [[클래스(Class)]] 사이의 중간 매개 역할까지 담당하는 일종의 [[추상 클래스(Abstract Class)]]를 의미한다.

- 인터페이스는 클래스 내에 선언된 모든 메서드가 [[추상 메서드(Abstract Method)]]인 클래스를 의미한다.  
- 이 인터페이스를 상속받는 클래스는 인터페이스에서 정의된 추상 메서드를 모두 구현해야 한다.

## 클래스로 다중상속을 하게 될 시의 문제점

```java
public class Son extends Father, Mother{ // 다중 상속 미지원
	...
}
```

- 위의 형태와 같이 자바는 여러 [[부모 클래스(super class)]]에서 [[자식 클래스(sub class)]]로 상속을 하는 [[다중상속]]을 지원하지 않는다.([[오버로딩(Overloading)]]의 개념과 다르다.) 
- [[자식 클래스(sub class)]]가 여러 [[부모 클래스(super class)]]를 상속받을 수 있다면, 다양한 동작을 수행할 수 있다는 장점을 가지게 될 것이다.
- 클래스를 이용하여 다중 상속을 할 경우 [[메서드(Method)]] 출처의 모호성 등 여러 가지 문제가 발생할 수 있어 자바에서는 클래스를 통한 다중 상속은 지원하지 않는다.



- 다중 상속의 이점을 버릴 수는 없기에 자바에서는 인터페이스라는 것을 통해 다중 상속을 지원하고 있다.
- 자바에서 [[추상 클래스(Abstract Class)]]는 [[추상 메서드(Abstract Method)]]뿐만 아니라 [[생성자(constructor)]], [[필드(field)]], 일반 [[메서드(Method)]]도 포함할 수 있다.
- 하지만 인터페이스는 오로지 추상 메서드와 상수만을 포함할 수 있다.

### 인터페이스의 선언

- 자바에서 인터페이스를 선언하는 방법은 클래스를 작성하는 방법과 같다.
- 인터페이스를 선언할 때에는 [[접근제어자]]와 함께 interface 키워드를 사용하면 된다.


- 자바에서 인터페이스는 다음과 같이 선언합니다.
### 문법

```java
접근제어자 interface 인터페이스이름 {

    public static final 타입 상수이름 = 값;

    ...

    public abstract 메서드이름(매개변수목록);

    ...

}
```

- 단, 클래스와는 달리 인터페이스의 모든 필드는 [[public]] [[static]] final이어야 하며, 모든 메소드는 public abstract이어야 한다.

- 이 부분은 모든 인터페이스에 공통으로 적용되는 부분이므로 이 제어자는 생략할 수 있다.
- 이렇게 생략된 제어자는 컴파일 시 자바 컴파일러가 자동으로 추가해 준다.

### 인터페이스의 구현
- 인터페이스는 [[추상 클래스(Abstract Class)]]와 마찬가지로 자신이 직접 [[인스턴스(Instance)]]를 생성할 수는 없다.
- 따라서 인터페이스가 포함하고 있는 추상 메소드를 구현해 줄 클래스를 작성해야만 한다.

- 자바에서 인터페이스는 아래와 같은 문법을 통해 구현한다.
#### 문법

```java
class 클래스이름 implements 인터페이스이름 { ... }
```

- 만약 모든 추상 메소드를 구현하지 않는다면, abstract 키워드를 사용하여 추상 클래스로 선언해야 한다.

- 다음 에시 코드는 인터페이스를 구현하는 예시이다.
##### 예시

```java
interface Animal { // 인터페이스 선언
	public abstract void cry(); // 인터페이스 안에 메서드는 abstract 키워드로 선언
}

class Cat implements Animal { // 상속받아서
    public void cry() {
        System.out.println("냐옹냐옹!"); // 내용을 오버라이딩
    }
}

class Dog implements Animal {
    public void cry() {
        System.out.println("멍멍!");
    }
}

public class Polymorphism03 {
    public static void main(String[] args) {
        Cat c = new Cat();
        Dog d = new Dog();
        c.cry();
        d.cry();
    }
}

>>냐옹냐옹!
>>멍멍! 
```

- 따라서 자바에서는 다음과 같이 상속과 구현을 동시에 할 수 있다.

#### 문법

```java
class 클래스이름 extend 상위클래스이름 implements 인터페이스이름 { ... }
```

- 인터페이스는 인터페이스로부터만 상속을 받을 수 있으며, 여러 인터페이스를 상속받을 수 있다.
- 밑의 코드는 인터페이스를 사용한 다중 상속의 예시이다.

##### 예제
```java
interface Animal { 
	public abstract void cry(); 
}

interface Pet { 
	public abstract void play(); 
}

class Cat implements Animal, Pet { // implements 키워드를 이용한 상속은 가능

    public void cry() {
        System.out.println("냐옹냐옹!");
    }

    public void play() {
        System.out.println("쥐 잡기 놀이하자~!");
    }
}

class Dog implements Animal, Pet {

    public void cry() {
        System.out.println("멍멍!");
    }

    public void play() {
        System.out.println("산책가자~!");
    }
}

public class Polymorphism04 {
    public static void main(String[] args) {
        Cat c = new Cat();
        Dog d = new Dog();
        c.cry();
        c.play();
        d.cry();
        d.play();
    }
}

>>냐옹냐옹!
>>나비야~ 쥐 잡기 놀이하자~!
>>멍멍!
>>바둑아~ 산책가자~!
```

- 위의 예제에서 Cat 클래스와 Dog 클래스는 각각 Animal과 Pet이라는 두 개의 인터페이스를 동시에 구현하고 있다.

#### 클래스를 이용한 다중 상속의 문제점

- 클래스를 이용하여 [[다중상속]]을 하면 다음 예제와 같은 메서드 출처의 모호성 등의 문제가 발생할 수 있다.
#### 예시
```java
class Animal { // 클래스 선언
    public void cry() {
        System.out.println("짖기!");
    }
}

class Cat extends Animal { // 클래스 상속
    public void cry() {
        System.out.println("냐옹냐옹!");
    }
}

class Dog extends Animal { 
    public void cry() {
        System.out.println("멍멍!");
    }
}

class MyPet extends Cat, Dog {} // ①


public class Polymorphism { 
    public static void main(String[] args) {
        MyPet p = new MyPet();
        p.cry(); // ②
    }
}
```

- 위의 예제에서 Cat 클래스와 Dog 클래스는 각각 Animal 클래스를 상속받아 cry() 메소드를 [[오버라이딩(Overriding)]]하고 있다.

- 여기까지는 문제가 없지만, ①번 라인에서 MyPet 클래스가 Cat 클래스와 Dog 클래스를 동시에 상속받게 되면 문제가 발생한다.

- ②번 라인에서 MyPet 인스턴스인 p가 cry() 메소드를 호출하면, 이 메소드가 Cat 클래스에서 상속받은 cry() 메소드인지 Dog 클래스에서 상속받은 cry() 메소드인지를 구분할 수 없는 모호성을 지니게 된다.

- 이와 같은 이유로 자바에서는 클래스를 이용한 다중 상속을 지원하지 않는다.

- 하지만 다음 예시처럼 인터페이스를 이용하여 다중 상속을 하게되면, 위와 같은 메사드 호출의 모호성을 방지할 수 있다.

#### 예시

```java
interface Animal { public abstract void cry(); }

interface Cat extends Animal { public abstract void cry(); }

interface Dog extends Animal { public abstract void cry(); }

class MyPet implements Cat, Dog {

    public void cry() {

        System.out.println("멍멍! 냐옹냐옹!");

    }

}

public class Polymorphism05 {

    public static void main(String[] args) {

        MyPet p = new MyPet();

        p.cry();

    }

}
```

##### 실행 결과

멍멍! 냐옹냐옹!

위의 예제에서는 Cat 인터페이스와 Dog 인터페이스를 동시에 구현한 MyPet 클래스에서만 cry() 메소드를 정의하므로, 앞선 예제에서 발생한 메소드 호출의 모호성이 없다.

#### 인터페이스의 장점

인터페이스를 사용하면 다중 상속이 가능할 뿐만 아니라 다음과 같은 장점을 가질 수 있다.

1. 대규모 프로젝트 개발 시 일관되고 정형화된 개발을 위한 표준화가 가능하다.

2. 클래스의 작성과 인터페이스의 구현을 동시에 진행할 수 있으므로, 개발 시간을 단축할 수 있다.

3. 클래스와 클래스 간의 관계를 인터페이스로 연결하면, 클래스마다 독립적인 프로그래밍이 가능하다.