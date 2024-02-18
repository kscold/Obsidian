- getClass() 이용하여 특정 객체의 Class로 접근 시에 getDeclaredFields()로 내부의 속성(Property)들의 이름을 getDeclaredFields() [[메서드(Method)]]를 이용하여 Field 객체의 [[배열(Array)]] 형태로 가져올 수 있다.

## 예시

- 보통 내부 클래스에서는 [[리플렉션(Reflection)]]을 이용해서 변수를 참조하지 않는다. 
- 내부 클래스이므로 굳이 필요성이 없다.
- 또는 외부에서 클래스를 찾거나 [[메서드(Method)]]를 찾아서 사용할 때에는 [[캡슐화(encapsulation)]]의 방법으로 [[멤버 변수(Nember Variable)]]에 [[private]] 처리를 한다.

```java
import java.lang.reflect.Field;

class Node {
	private int data = 0; // 멤버 변수 설정
	
	public void setData(int data) {
		this.data = data;
	}
	
	// 멤버 변수 리턴
	public int getData() {
		return this.data;
	}
}

public class Example {
	// 실행 함수
	public static void main(String... args) {
		try {
			// 인스턴스 생성
			Node node = new Node();
			
			// Reflection으로 data 변수를 취득
			Field field = Node.class.getDeclaredField("data");
			
			// setAccessible는 private, protected도 접근 가능함
			field.setAccessible(true);
			
			// data 필드에 100를 넣음
			field.set(node, 100);
			
			// 결과는 100이 나옴
			System.out.println(node.getData());
			
		} catch (Throwable e) {
			e.printStackTrace();
		}
	}
}
```

- 그러나 위 예시에서는 private 타입으로 된 data 변수를 취득했고, 데이터를 입력하기까지 했다.

- 즉, [[리플렉션(Reflection)]]을 이용하면 [[JUnit5]] 등과 같은 [[테스트 메서드(Test Method)]]을 작성할 때, 작성되어 있는 함수의 결과 값만 확인하는 것이 아니라 중간 중간 클래스의 [[멤버 변수(Nember Variable)]] 값을 확인할 수 있다.