  - 정적 메서드는 [[객체(Object)]]([[인스턴스(Instance)]])의 생성 없이 호출이 가능하며, 객체에서는 호출이 가능은 하지만 권장하지는 않는다.

  - 일반적으로는 유틸리티 관련 함수들은 여러 번 사용되므로 [[static]] [[메서드(Method)]]로 구현을 하는 것이 적합한데, static 메서드를 사용하는 대표적인 Util Class로는 java.uitl.Math가 있다.
  
## 정적 메서드를 사용하는 이유

1. [[인스턴스(Instance)]] 생성 없이 호출이 가능하다.(인스턴스 생성 후 호출도 가능하지만 지양하고 있다.)  
2. 유틸리티 관련 함수를 만드는데 유용하게 사용된다.

- 1번의 의미를 코드를 통해 먼저 확인하자.

```java
public class Test {     
	public static void sm() {        
		System.out.println("this is static method!");    
	} 
	
	public void m() {       
		System.out.println("this is non-static method!");    
	}
} 

public class Main {    
	public static void main(String[] args) {  
			Test.sm(); // O        
			Test.m(); // X
			
			Test test = new Test();        
			test.sm(); // X        
			test.m(); // O    
	}
}
```

- 위처럼 가능한 이유는 말 그대로 정적이기 때문에, 클래스가 메모리에 올라갈 때 정적 메서드가 자동적으로 생성된다.
- 그렇기에 [[인스턴스(Instance)]]를 생성하지 않고, 클래스만으로 메소드를 호출할 수 있는 것이다.

- 2번의 대표적인 유틸리티 클래스는 java.lang.Math 가 있다.

```java
Math.max()
Math.min()
// .. 등
```

- Math 클래스도 [[클래스(Class)]]만으로 [[메서드(Method)]]를 호출하고 있다.
- 여기서 max,min 같은 메소드는 정적 메소드로 구현되어 있으며, 유틸리티 클래스는 상태를 가지고 있지 않은 클래스라고 보면 된다.

- Math 클래스를 들여다보면 다음과 같다.

```java
public final class Math {    
	...    
	public static final double PI = 3.14159265358979323846;    
	...        
	
	public static int max(int a, int b) {        
		return (a >= b) ? a : b;    
	}     
	
	...
	
	public static int min(int a, int b) {       
		return (a <= b) ? a : b;    
	}   
	
	...
}
```

## 정적 메서드는 무슨 기준으로 정해야 하는가?

- 한 문장으로 말하면 인스턴스를 생성하지 않고 호출할 할 것이면 정적 메서드로 선언하고 정적으로 보면 된다.
	1. 변화를 가정하지 않는다.
	2. 메서드가 [[인스턴스 변수(Instance Variable)]]를 사용하지 않는다.

- 인스턴스 생성에 의존하지 않는다.
- 메서드가 공유되고 있다면, 정적 메서드로 추출해낼 수 있다.
- 메서드가 변화되지 않고, [[오버라이딩(Overriding)]] 되지 않는다.

## 유틸리티 클래스 활용 예시

```java
public final class CommonUtils {    
	public static LocalDateTime stringToLocalDateTime(String strDate) {
		return LocalDateTime.parse(strDate, DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
	}     
	
	public static String localDateTimeToString(LocalDateTime localDateTime) {        
		return localDateTime.format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));    
	}     
	
	public static boolean isEmailValid(String email) {        
		return Patterns.EMAIL_ADDRESS.matcher(email).matches();    
	}
}
```

- 상속을 방지하기 위해 [[final]] class로 선언하고, 유틸 관련된 함수들을 모아둔다.