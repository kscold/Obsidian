- 한마디로 소프트웨어 세계에 구현할 대상이다.

- 객체는 [[클래스(Class)]]의 [[인스턴스(Instance)]]이다. 
- 또한 클래스를 기반으로 생성된 실제 데이터 구조이다.

- 객체는 클래스에서 정의된 필드에 실제 값이 채워진 것이다.
- 즉, 객체는 메모리에서 실제로 존재하고 데이터를 가진다.

- [[클래스(Class)]]에 선언된 모양 그대로 생성된 실체, 인스턴스이긴 하지만 실제 우리가 칭하는 인스턴스처럼 객체에 값을 대입하지 않은 인스턴스이다.
- 객체는 모든 인스턴스를 대표하는 포괄적인 의미를 갖는다.

## 예시

```java
public class Animal { // class 객체를 생성하는 연산자
  ...
}

/* 객체와 인스턴스(Instance) */
public class Main {
  public static void main(String[] args) {
    Animal cat, dog; // `Animal` 클래스의 인스턴스를 참조할 변수를 선언 -> 형식을 명시해 준비

	// 인스턴스화
	cat = new Animal(); // cat은 Animal 클래스의 '인스턴스'(객체를 메모리에 할당)
    dog = new Animal(); // dog은 Animal 클래스의 '인스턴스'(객체를 메모리에 할당)
  }
}
```

- 여기서 new Animal() 코드는 컴퓨터 메모리에 클래스를 복사해서 빈값의 인스턴스를 선언한 것이다.


## 메모리의 객체

```java
public class Test {
	String str = "test";
    
    public void setTest(String str) {
    	this.str = str
    }
}
```

- 위의 간단한 Test 객체가 있다.
- 이 객체는 현재 자바 파일로 존재할 뿐이지 실제로 사용되진 않은 라이브러리 객체이기때문에 JVM의 메모리 영역에 올라가있진 않다.

### 객체 생성

```java
Test t1 = new Test();
```

- 어디선가 Test 객체를 생성([[new]])했다.

- 이 객체는 이제 JVM 메모리 어딘가에 생성되었다는 뜻이고, 메모리 영역 중 힙 영역(Heap Area)이다. 
- 즉, 힙 영역은 객체가 생성되는 공간이다.


