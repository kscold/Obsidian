- 힙 영역에 로드돼 있는 클래스 타입의 객체를 통해 [[Java/필드(Field)|필드(Field)]]//[[메서드(Method)]]/[[생성자(constructor)]]를 [[접근제어자(Access modifier)]]와 상관 없이 사용할 수 있도록 지원하는 API이다.

- 컴파일 시점이 아닌 런타임 시점에 동적으로 특정 [[클래스(Class)]]의 정보를 추출해낼 수 있는 프로그래밍기법이다.
- 주로 프레임워크 또는 라이브러리 개발 시 사용된다.


## Class`<?>`

- 리플렉션에서 `<?>`는 와일드카드를 나타내며, 어떤 유형의 클래스든 허용한다는 것을 의미한다.
- 즉, [[클래스(Class)]] [[객체(Object)]]가 어떤 클래스인지 정확히 알 필요가 없을 때 사용된다.

- 리플렉션에서 Class 객체를 사용하면 해당 클래스의 구조를 조사하고 수정할 수 있다.
- 이는 실행 중에 클래스의 메서드, 필드, 생성자 등에 접근할 수 있는 강력한 기능을 제공한다.

- 밑의 코드는 Class`<?>`를 사용하여 클래스의 메서드를 가져오는 간단한 예시이다.

```java
import java.lang.reflect.Method; 

public class Main {    
	public static void main(String[] args) throws NoSuchMethodException {        
		Class<?> clazz = MyClass.class; // MyClass 클래스의 모든 메서드 가져옴     
		Method[] methods = clazz.getDeclaredMethods();         
		
		for (Method method : methods) {             
			System.out.println("Method name: " + method.getName());         
		}     
	} 
}  

class MyClass {     
	public void method1() { 
		// 메서드 내용     
	}
	
	public int method2(String str) { 
		// 메서드 내용         
		return str.length();     
	} 
}
```

## 리플렉션(Reflection) 메서드

### Reflections

- Reflections 생성자에 넘겨주는 파라미터 값은 클래스를 찾을때의 출발 패키지이다. 
- 매개변수 값이 "com.atoz_develop"이면 com.atoz_develop 패키지와 그 하위 패키지를 모두 찾는다. 
- 빈 문자열을 넘기면 자바 classpath에 있는 모든 패키지를 찾는다.

### [[getDeclaredFields()]]

- 이 [[메서드(Method)]]를 통해 특정 객체의 [[클래스(Class)]]로 접근 시에 내부의 속성(Property)들의 이름을 Field 객체의 [[배열(Array)]] 형태로 가져올 수 있다.
### getDeclaredConstructors()

- 비슷하게 리플렉션을 활용하여 [[클래스(Class)]]의 내부 [[생성자(constructor)]]의 정보를 배열 형태로 가져올 수 있다.
### getDeclaredMethods()

- 비슷하게 리플렉션을 활용하여 [[클래스(Class)]]의 내부 [[메서드(Method)]]의 정보를 배열 형태로 가져올 수 있다.
### getTypesAnnotatedWith()

- 이 [[메서드(Method)]]를 호출할때 [[어노테이션(Annotation)]]의 [[클래스(Class)]]를 매개변수로 넘겨 해당 어노테이션이 붙은 [[클래스(Class)]]를 찾을 수 있다.
- getTypesAnnotatedWith(Component.class)와 같이 호출하면 @Component 어노테이션이 붙은 클래스를 찾는다.
- 반환값은 해당 어노테이션이 선언된 클래스 목록이다.

### getAnnotation().value()

- getAnnotation()을 통해 어노테이션을 추출한다.
- 어노테이션을 추출하면 해당 어노테이션에 정의된 메서드(속성)을 호출해서 속성 값을 꺼낼 수 있다.

- Component 어노테이션에는 value() 속성이 정의되어 있으므로 value()를 호출해서 value()에 설정된 값을 꺼낼 수 있다.
- 이렇게 꺼낸 값을 key로 하여 필요한 곳에서 사용할 수 있도록 객체를 저장한다.

## 리플렉션(Reflection)을 사용하는 프레임워크/라이브러리

### Spring 프레임워크

- [[DI(Dependency Injection)]]를 예시로 들 수 있다.

## Test 프레임워크

- [[JUnit5]]를 예시로 들 수 있다.

## [[JSON(Java Script Object Notation)]] Serialization/Deserializatin 라이브러리

- Jackson 라이브러리를 예시로 들 수 있다.