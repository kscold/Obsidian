- [[멤버 변수]]라고도 부른다.
- [[인스턴스(Instance)]]가 가지는 고유한 상태를 유지해야하는 속성을 저장하기 위한 [[변수(Variable)]]이다.
- [[패키지(package)]] 내에서만 접근 가능한 멤버로 간주한다.
- 따라서 [[new]] [[메서드(Method)]]()를 통해 인스턴스가 생성될 때(원래 [[클래스(Class)]]의 [[생성자(constructor)]]도 시작된다.)만들어진다.
- 따라서 인스턴스를 생성한 뒤, ‘인스턴스명.인스턴스변수명’을 통해 사용할 수 있다.

- 인스턴스는 힙 메모리(트리 형태)의 독립적인 공간에 저장되고, 동일한 클래스로부터 생성되었지만 객체의 고유한 개별성을 가진다.
- [[private]] 키워드를 사용하여 변수를 [[캡슐화(encapsulation)]]할 수 있다.
- 클래스 블록에 변수를 선언할 때 [[접근제어자(Access modifier)]]를 생략할 경우에는 [[default]]로 간주된다.
- [[멤버 변수]]에 포함된다.

- 외부에서 인스턴스 변수에 쓰기를 할려면 [[Getter and Setter]] 에서 Setter를 이용하면 된다.


```java
class Variable { // public, private가 없으므로 패키지 내에서만 접근 가능
    int instanceVariable = 10; // 접근제어자가 없으면 default(패키지 내에서만 접근 가능)로 간주된다. 클래스 내에 인스턴스 변수 선언
    
    public static void main(String[] args) {
        Variable instance = new Variable(); // 인스턴스 생성
        System.out.println(instance.instanceVariable); // 10 출력
        // 인스턴스.인스턴스변수명을 명시하여 인스턴스변수를 호출
    }
}
```


- 밑의 코드는 private로 선언된 인스턴스 변수의 예시이다.

```java
public class MyClass {
    private int myPrivateVariable; // private 인스턴스 변수
    
	public int getMyVariable() { // Getter Setter 선언
        return this.myPrivateVariable; // 인스턴스에서 값 읽기 가능
    }
	
	public void setMyVariable(int value) { 
        this.myPrivateVariable = value; // 인스턴스에서 값 쓰기 가능
    }
}
```
