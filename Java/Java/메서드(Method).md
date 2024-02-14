[[클래스(Class)]] 내부에 선언된 함수라는 뜻이다.

- 메소드는 어떠한 작업을 수행하는 코드를 하나로 묶어 놓은 것이다.(대표적으로 CURD의 메소드)

- 메소드 내의 변수는 지역변수로, 메소드 내부에서만 사용할 수 있다.

- [[생성자(constructor)]]도 하나의 메소드다.

코드 컨벤션으로는, 시작은 동사로하되 camel case를 적용한다.

예시)
```java
public class Function { // 생성자 Funtion이 작동
	public static int add(int number1, int number2){ // 클래스 메소드
		int result = x + y;
		return result;
	}


	public static void main(String[] args) { // 클래스 메소드
		int result = add(10, 5);

		System.out.println("더하기 값은: " + result + "입니다.");
	}
}

// 결과 
15
```

매소드는 크게 [[클래스 메서드(Class Method)]]와 [[인스턴스 메서드(instance method)]]로 나눌 수 있다.
