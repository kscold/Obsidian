
- [[static]] 키워드([[클래스 메서드(Class Method)]])를 가지지 않는 [[메서드(Method)]]를 인스턴스 메소드(instance method)라고 한다.

## 예시

- 밑의 예시는 인스턴스 메서드와 [[클래스 메서드(Class Method)]]의 차이를 보여주는 코드이다.

```java
class Method {
	int a = 10, b = 20;                            // 인스턴스 변수
	int add() { return a + b; }                    // 인스턴스 메서드
	static int add(int x, int y) { return x + y; } // 클래스 메서드(정적 메서드)
}

public class Member02 {
	public static void main(String[] args) {
		System.out.println(Method.add(20, 30)); // 클래스 메소드의 호출
		
		Method myMethod = new Method();         // 인스턴스 생성
		
		System.out.println(myMethod.add());     // 인스턴스 메서드의 호출
	}
}

// >> 50
// >> 30
```
