
- 자바에서는 객체의 생성과 동시에 [[인스턴스 변수]]를 원하는 값으로 초기화할 수 있는 생성자(constructor)라는 메소드를 제공한다.

- 자바에서 생성자(constructor)의 이름은 해당 [[클래스(Class)]] 이름과 같아야 한다.

- [[this]]와 [[super]] 역시 생성자이다.

즉, Car 클래스의 생성자의 이름은 Car가 된다.

이러한 생성자는 다음과 같은 특징을 가진다.

1. 생성자는 반환값이 없지만, 반환 타입을 void형으로 선언하지 않는다.

2. 생성자는 초기화를 위한 데이터를 인수로 전달받을 수 있다.

3. [[객체(Object)]]를 초기화하는 방법이 여러 개 존재할 경우에는 하나의 클래스가 여러 개의 생성자를 가질 수 있습니다.

즉, 생성자도 하나의 [[메소드(Method)]]이므로, 메소드 오버로딩이 가능하다는 의미이다.

#### 생성자의 선언 문법

자바에서 클래스 생성자를 선언하는 문법은 다음과 같다.

1. 클래스이름() { ... }                  // 매개변수가 없는 생성자 선언

2. 클래스이름(인수1, 인수2, ...) { ... } // 매개변수가 있는 생성자 선언


위와 같이 생성자 중에는 매개변수를 전달받아 인스턴스 변수를 초기화하는 생성자도 선언할 수 있다.

밑의 코드는 앞서 살펴본 Car 클래스의 생성자를 선언하는 예시

```java

Car(String modelName, int modelYear, String color, int maxSpeeds) {

    this.modelName = modelName; // 인스턴스 변수도 생성자로 미리 선언 String형으로 매치

    this.modelYear = modelYear;

    this.color = color;

    this.maxSpeed = maxSpeed;

    this.currentSpeed = 0;

}
```


밑의 코드는 Car 클래스를 선언하면서 여러 개의 생성자를 선언하는 예시

```java
// 상위에 public class가 선언되어 있다고 가정
Car(String modelName) {} // 클래스의 생성자를 정의, 다양한 방식

Car(String modelName, int modelYear) {}

Car(String modelName, int modelYear, String color) {}

Car(String modelName, int modelYear, String color, int maxSpeeds) {}
```

#### 생성자의 호출

자바에서는 [[new]] 키워드를 사용하여 객체를 생성할 때 자동으로 생성자가 호출됩니다.

밑에는 전체적인 new를 포함한 생성자 활용 예시

```java
class Car {
    private String modelName; // private 변수도 인스턴스 변수이기 때문에 미리 정의
    private int modelYear;
    private String color;
    private int maxSpeed;
    private int currentSpeed;
    
    Car(String modelName, int modelYear, String color, int maxSpeed) {
        this.modelName = modelName; // 생성자로 인스턴스 변수들을 정의
        this.modelYear = modelYear;
        this.color = color;
        this.maxSpeed = maxSpeed;
        this.currentSpeed = 0;
    }

    public String getModel() {
        return this.modelYear + "년식 " + this.modelName + " " + this.color;
    }
}

public class Method02 {
    public static void main(String[] args) { // main 클래스 메소드
        Car myCar = new Car("아반떼", 2016, "흰색", 200); // 생성자의 호출
        System.out.println(myCar.getModel()); // 생성자에 의해 초기화되었는지를 확인함.
    }
}

// 실행 결과

2016년식 아반떼 흰색
```

`Car` 클래스에 다양한 생성자를 선언하는 것은 가능하다. 이것은 오버로딩(Overloading)이라고 알려진 프로세스이다. 오버로딩을 사용하면 동일한 메소드(생성자 포함) 이름을 가지고 있지만 서로 다른 매개변수 목록을 사용하여 여러 버전의 생성자를 정의할 수 있다. 따라서 클래스에 다음과 같이 여러 생성자를 선언하는 것은 가능하다.

#### 자바에 생성자가 필요한 이유

- 인스턴스 변수의 초기화

[[클래스(Class)]]를 가지고 객체를 생성하면, 해당 [[객체(Object)]]는 메모리(메타데이터)에 즉시 생성된다.

하지만 이렇게 생성된 객체는 모든 인스턴스 변수가 아직 초기화되지 않은 상태이다.


```java
public class Animal { // class 객체를 생성하는 연산자
  ...
}

/* 객체와 인스턴스(Instance) */
public class Main {
  public static void main(String[] args) { // 클래스 메소드
    Animal cat, dog; // `Animal` 클래스의 인스턴스를 참조할 변수를 선언 -> 형식을 명시해 준비
	// 객체를 명시만 했지 인스턴스 변수가 초기화되지 않은 상태
	
	// 인스턴스화
	cat = new Animal(); 
    dog = new Animal(); 
  }
}
```

자바에서 클래스 변수와 인스턴스 변수는 별도로 초기화하지 않으면, 밑에 값처럼 초기화 된다.

|변수의 타입|초깃값|
|---|---|
|char|'\u0000'|
|byte, short, int|0|
|long|0L|
|float|0.0F|
|double|0.0 또는 0.0D|
|boolean|false|
|배열, 인스턴스 등|null|

하지만 사용자가 원하는 값으로 인스턴스 변수를 초기화하려면, 일반적인 초기화 방식으로는 초기화할 수 없다.

인스턴스 변수 중에는 [[private]] 변수도 있으며, 이러한 private 변수에는 사용자나 프로그램이 직접 접근할 수 없기 때문이다.

따라서 private 인스턴스 변수에도 접근할 수 있는 초기화만을 위한 [[public]] 메소드가 필요해집니다.

이러한 초기화만을 위한 메소드는 객체가 생성된 후부터 사용되기 전까지 반드시 인스턴스 변수의 초기화를 위해 호출되어야 한다.


#### 기본 생성자(default constructor)

자바의 모든 클래스에는 하나 이상의 생성자가 정의되어 있어야 한다.

하지만 특별히 생성자를 정의하지 않고도 인스턴스를 생성할 수 있다.

이것은 자바 컴파일러가 기본 생성자(default constructor)라는 것을 기본적으로 제공해 주기 때문이다.

기본 생성자는 매개변수를 하나도 가지지 않으며, 아무런 명령어도 포함하고 있지 않다.

자바 컴파일러는 컴파일 시 클래스에 생성자가 하나도 정의되어 있지 않으면, 자동으로 다음과 같은 기본 생성자를 추가합니다.

- 기본 생성자 사용 분법

클래스이름() {}



밑에는 자바 컴파일러가 Car 클래스에 자동으로 추가해 주는 기본 생성자의 예시

```java
Car() {}
```

위와 같이 기본 생성자는 어떠한 매개변수도 전달받지 않으며, 기본적으로 아무런 동작도 하지 않는다.

밑의 코드는 Car 클래스에 생성자를 정의하지 않고, 기본 생성자를 호출하는 예시

```java
class Car {
    private String modelName = "소나타";
    private int modelYear = 2016;
    private String color = "파란색";
    
    public String getModel() {
        return this.modelYear + "년식 " + this.color + " " + this.modelName;
    }
}

public class Method03 {
    public static void main(String[] args) {
        Car myCar = new Car();                // 기본 생성자의 호출
        System.out.println(myCar.getModel()); // 2016년식 파란색 소나타
    }
}

// 실행 결과

2016년식 파란색 소나타

```

오류가 나는 예시

```java
class Car {
    private String modelName;
    private int modelYear;
    private String color;
    private int maxSpeed;
    private int currentSpeed;

	Car(String modelName, int modelYear, String color, int maxSpeed) {
		this.modelName = modelName;
        this.modelYear = modelYear;
        this.color = color;
        this.maxSpeed = maxSpeed;
        this.currentSpeed = 0;
    }

    public String getModel() {
	    return this.modelYear + "년식 " + this.modelName + " " + this.color;
    }
}

public class Method04 {
	public static void main(String[] args) {
	    Car myCar = new Car(); // 기본 생성자의 호출 -> 기본적으로 표현할 값이 없음
	    Car myCar = new Car("아반떼", 2016, "흰색", 200);
	    // 생성자의 호출, 이 코드만 실행시킨다면 오류가 안남
	    
	    System.out.println(myCar.getModel()); // 생성자에 의해 초기화되었는지를 확인함.
	}
}
```