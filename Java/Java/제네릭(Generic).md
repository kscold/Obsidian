- 제네릭(Generic)은 직역하자면 '일반적인'이라는 뜻이다. 
- 제네릭(Generic)은 클래스 내부에서 지정하는 것이 아닌 외부에서 사용자에 의해 지정되는 것을 의미한다. 
- 한마디로 특정(Specific) 타입을 미리 지정해주는 것이 아닌 필요에 의해 지정할 수 있도록 하는 일반(Generic) 타입이라는 것이다.

- 조금 더 부연설명을 하자면 '데이터 형식에 의존하지 않고, 하나의 값이 여러 다른 데이터 타입들을 가질 수 있도록 하는 방법'이다.

- 우리가 흔히 쓰는 ArrayList, LinkedList 등을 생성할 때 어떻게 쓰는가?


## 제네릭 생성방식

- 객체<타입> 객체명 = new 객체<타입>(); 즉, 아래와 같이  [[ArrayList]], [[LinkedList]] 등 여러 생성방식이 있다.

```java
ArrayList<Integer> list1 = new ArrayList<Integer>();
ArrayList<String> list2 = new ArrayList<Integer>();

LinkedList<Double> list3 = new LinkedList<Double>();
LinkedList<Character> list4 = new LinkedList<Character>();
```

- 이렇게 <> 괄호 안에 들어가는 타입을 지정해준다.

## 제네릭의 장점

1. 제네릭을 사용하면 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있다.
2. [[클래스(Class)]] 외부에서 타입을 지정해주기 때문에 따로 타입을 체크하고 변환해줄 필요가 없다.(즉, 관리하기가 편하다.)
3. 비슷한 기능을 지원하는 경우 코드의 재사용성이 높아진다.

## 제네릭 사용방법

  - 보통 제네릭은 아래 표의 타입들이 많이 쓰인다. 
  
| 타입 | 설명 |
| ---- | ---- |
| `<T>` | Type |
| `<E>` | Element |
| `<K>` | Key |
| `<V>` | Value |
| `<N>` | Number |

- 물론 반드시 한 글자일 필요는 없다.
- 또한 설명과 반드시 일치해야 할 필요도 없지만 대중적으로 통하는 통상적인 선언이 가장 편하기 때문에 위와같은 암묵적인 규칙 표기법이다.

### 1. 클래스 및 인터페이스 선언

```java
public class ClassName <T> { ... }
public class ClassName <T> T { 
	public <T> T Method() {
		
	}	
}

public interface InterfaceName <T> { ... }
```

- 기본적으로 제네릭 타입의 [[클래스(Class)]]나 [[인터페이스(Interface)]]의 경우 위와같이 선언한다.
- T 타입은 해당 블럭 { ... } 안에서까지 유효하다.
- 또한 `<T>` 뒤에 `T`를 한 번 더 붙여주는 것은 제네릭 타입 매개변수를 반환 타입으로 사용하겠다는 것을 의미한다.
- 즉, [[메서드(Method)]]가 제네릭 타입을 사용하고 있고, 이 메서드는 해당 제네릭 타입을 반환할 것임을 나타낸다.


- 또한 여기서 더 나아가 제네릭 타입을 두 개로 둘 수도 있다. (대표적으로 타입 인자로 두 개 받는 대표적인 컬렉션인 [[HashMap]]을 생각해보자.)

```java
public class ClassName <T, K> { ... }

public interface InterfaceName <T, K> { ... }

public class HashMap <K, V> { ... }  // HashMap의 경우
```

- 이렇듯 데이터 타입을 외부로부터 지정할 수 있도록 할 수 있다.
- 그럼 이렇게 생성된 제네릭 클래스를 사용하고 싶을 것이다.
- 즉, [[객체(Object)]]를 생성해야 하는데 이 때 구체적인 타입을 명시를 해주어야 하는 것이다.

```java
public class ClassName <T, K> { ... } 

public class Main {	
	
	public static void main(String[] args) {		
		ClassName<String, Integer> a = new ClassName<String, Integer>();	
	}
}
```

- 위 예시대로라면 T는 String이 되고, K는 Integer가 된다.
- 이 때 주의해야 할 점은 타입 매개변수로 명시할 수 있는 것은 [[참조 타입(Reference Type)]]밖에 올 수 없다. 
- 즉, int, double, char 같은 [[원시 타입(Primitive Type)]]은 올 수 없다는 것이다. 

- 그래서 int형 double형 등 [[원시 타입(Primitive Type)]]의 경우 Integer, Double 같은 Wrapper Type으로 쓰는 이유가 바로 위와같은 이유다.

- 또한 바꿔 말하면 [[참조 타입(Reference Type)]]이 올 수 있다는 것은 사용자가 정의한 [[클래스(Class)]]도 타입으로 올 수 있다는 것이다.

```java
public class ClassName <T> { ... } 

public class Student { ... } 

public class Main {

	public static void main(String[] args) {	
		ClassName<Student> a = new ClassName<Student>();	
	}
}
```

### 2. 제네릭 클래스

- 그러면 [[클래스(Class)]] 및 [[인터페이스(Interface)]]를 제네릭으로 받는 방법을 알아봤으니 본격적으로 활용해보자.

```java
// 제네릭 클래스

class ClassName<E> {		
	private E element; // 제네릭 타입 변수		
	
	void set(E element) { // 제네릭 파라미터 메소드		
		this.element = element;	
	}		
	
	E get() { // 제네릭 타입 반환 메소드		
		return element;	
	}
} 

class Main {	
	public static void main(String[] args) {	
		ClassName<String> a = new ClassName<String>();		
		ClassName<Integer> b = new ClassName<Integer>();	
		
		a.set("10");		
		b.set(10);			
		
		System.out.println("a data : " + a.get()); // 반환된 변수의 타입 출력 		
		System.out.println("a E Type : " + a.get().getClass().getName());				
		System.out.println();
		System.out.println("b data : " + b.get()); // 반환된 변수의 타입 출력 		
		System.out.println("b E Type : " + b.get().getClass().getName());			
	}
}
```

- 보면 ClassName이란 객체를 생성할 때 <> 안에 타입 매개변수(Type parameter)를 지정한다.
- 그러면 a객체의 ClassName의 E 제네릭 타입은 String으로 모두 변환된다.
- 반대로 b객체의 ClassName의 E 제네릭 타입은 Integer으로 모두 변환된다.

- 실제로 위 코드를 실행시키면 다음과 같이 출력된다.

![](https://blog.kakaocdn.net/dn/bwZf3n/btqLeqtwL2Z/juSEIt48ifvKRIPZ6PALL1/img.png)

- 만약 제네릭을 두 개 쓰고 싶다면 이렇게 할 수도 있다.

```java
// 제네릭 클래스 
class ClassName<K, V> {
	private K first;	// K 타입(제네릭)	
	private V second;	// V 타입(제네릭) 		
	
	void set(K first, V second) {		
		this.first = first;		
		this.second = second;	
	}
	
	K getFirst() {		
		return first;	
	}		
	
	V getSecond() {		
		return second;	
	}
} 

// 메인 클래스 
class Main {	
	
	public static void main(String[] args) {		
		ClassName<String, Integer> a = new ClassName<String, Integer>();
		a.set("10", 10);  		
		
		System.out.println("  fisrt data : " + a.getFirst()); // 반환된 변수의 타입 출력 
		System.out.println("  K Type : " + a.getFirst().getClass().getName());			
		System.out.println("  second data : " + a.getSecond());		// 반환된 변수의 타입 출력 
		System.out.println("  V Type : " + a.getSecond().getClass().getName());	
	}
}
```

- 결과는 다음과 같다.

![](https://blog.kakaocdn.net/dn/cOmFzN/btqLcU2HSy0/Ba7cK2T5szTxVrLzGKnKeK/img.png)

- 이렇게 외부 클래스에서 제네릭 클래스를 생성할 때 <> 괄호 안에 타입을 매개변수로 보내 제네릭 타입을 지정해주는 것이 제네릭 프로그래밍이다.

### 3. 제네릭 메서드

- 위 과정까지는 클래스 이름 옆에 예로들어 `<E>`라는 제네릭타입을 붙여 해당 클래스 내에서 사용할 수 있는 E 타입으로 일반화를 했다.
- 그러나 그 외에 별도로 메서드에 한정한 제네릭도 사용할 수 있다.

- 일반적으로 선언 방법은 다음과 같다. 

```java
public <T> T genericMethod(T o) {	// 제네릭 메소드	
	...
}

[접근 제어자] <제네릭타입> [반환타입] [메소드명]([제네릭타입] [파라미터]) {	
	// 텍스트
}
```

- 클래스와는 다르게 반환타입 이전에 <> 제네릭 타입을 선언한다.

- 위에서 다룬 제네릭 클래스에서 활용해보도록 하자.

```java
// 제네릭 클래스
class ClassName<E> {		
	private E element;	// 제네릭 타입 변수		
	
	void set(E element) {	// 제네릭 파라미터 메소드		
		this.element = element;	
	}		
	
	E get() {	// 제네릭 타입 반환 메소드 		
		return element;	
	}		
	
	<T> T genericMethod(T o) {	// 제네릭 메소드	
		return o;
	} 	
} 

public class Main {	
	public static void main(String[] args) {	
		ClassName<String> a = new ClassName<String>();		
		ClassName<Integer> b = new ClassName<Integer>();
		
		a.set("10");
		b.set(10);
		
		System.out.println("a data : " + a.get()); // 반환된 변수의 타입 출력 	
		System.out.println("a E Type : " + a.get().getClass().getName());			
		System.out.println();	
		System.out.println("b data : " + b.get()); // 반환된 변수의 타입 출력 	
		System.out.println("b E Type : " + b.get().getClass().getName());	
		System.out.println(); // 제네릭 메소드 Integer		
		System.out.println("<T> returnType : " + a.genericMethod(3).getClass().getName()); // 제네릭 메소드 String		
		
		System.out.println("<T> returnType : " + a.genericMethod("ABCD").getClass().getName()); // 제네릭 메소드 ClassName b		
		
		System.out.println("<T> returnType : " + a.genericMethod(b).getClass().getName());	}}
```

- 보면 ClassName이란 객체를 생성할 때 <> 안에 타입 파라미터(Type parameter)를 지정한다.
- 그러면 a객체의 ClassName의 E 제네릭 타입은 String으로 모두 변환된다.
- 반대로 b객체의 ClassName의 E 제네릭 타입은 Integer으로 모두 변환된다.

- genericMethod()는 파라미터 타입에 따라 T 타입이 결정된다.

- 실제로 위 코드를 실행시키면 다음과 같이 출력된다.

![](https://blog.kakaocdn.net/dn/czs31x/btq8zxKbZ05/ftKMjz295wRPDhCSa2UZtK/img.png)

- 즉, 클래스에서 지정한 제네릭유형과 별도로 메서드에서 독립적으로 제네릭 유형을 선언하여 쓸 수 있다.
- 그럼 위와같은 방식이 왜 필요한가? 바로 [[정적 메서드(Static Method)]]로 선언할 때 필요하기 때문이다.

- 앞서 제네릭은 유형을 외부에서 지정해준다고 했다. 
- 즉 해당 클래스 [[객체(Object)]]가 [[인스턴스(Instance)]]화 했을 때, 쉽게 말해 [[new]] 생성자로 클래스 [[객체(Object)]]를 생성하고 <> 괄호 사이에 파라미터로 넘겨준 타입으로 지정이 된다는 뜻이다.

- 하지만 [[static]]은 정적이라는 뜻이다. 
- static 변수, static 함수 등 static이 붙은 것들은 기본적으로 프로그램 실행시 메모리에 이미 올라가있다.

- 이 말은 객체 생성을 통해 접근할 필요 없이 이미 메모리에 올라가 있기 때문에 클래스 이름을 통해 바로 쓸 수 있다는 것이다.
- 근데, 거꾸로 생각해보자면 static 메소드는 객체가 생성되기 전에 이미 메모리에 올라가는데 타입을 어디서 얻어올 수 있을까? 

```java
class ClassName<E> { 	
/*	 
* 클래스와 같은 E 타입이더라도	 
* static 메소드는 객체가 생성되기 이전 시점에	
* 메모리에 먼저 올라가기 때문에	 
* E 유형을 클래스로부터 얻어올 방법이 없다.	 
* */	

	static E genericMethod(E o) {	// error!		
		return o;	
	}	
} 

class Main { 	
	public static void main(String[] args) { 		
	// ClassName 객체가 생성되기 전에 접근할 수 있으나 유형을 지정할 방법이 없어 에러남		
	ClassName.getnerMethod(3); 	
	}
}
```

- 그렇기 때문에 제네릭이 사용되는 메소드를 정적메소드로 두고 싶은 경우 제네릭 클래스와 별도로 독립적인 제네릭이 사용되어야 한다는 것이다.

```java
// 제네릭 클래스class ClassName<E> { 
	private E element; // 제네릭 타입 변수 
		void set(E element) { // 제네릭 파라미터 메소드		
		this.element = element;	
	} 	
	
	E get() { // 제네릭 타입 반환 메소드		
		return element;
	} 	// 아래 메소드의 E타입은 제네릭 클래스의 E타입과 다른 독립적인 타입이다.	
	
	static <E> E genericMethod1(E o) { // 제네릭 메소드		
		return o;	
	} 
	
	static <T> T genericMethod2(T o) { // 제네릭 메소드		
		return o;	
	}
} 

public class Main {	
	public static void main(String[] args) { 		
		ClassName<String> a = new ClassName<String>();		
		ClassName<Integer> b = new ClassName<Integer>(); 		
		
		a.set("10");		
		b.set(10); 		
		
		System.out.println("a data : " + a.get());		// 반환된 변수의 타입 출력		
		System.out.println("a E Type : " + a.get().getClass().getName()); 		
		System.out.println();		
		System.out.println("b data : " + b.get());		// 반환된 변수의 타입 출력		
		System.out.println("b E Type : " + b.get().getClass().getName());		
		System.out.println(); 		// 제네릭 메소드1 Integer		
		
		System.out.println("<E> returnType : " + ClassName.genericMethod1(3).getClass().getName()); 		// 제네릭 메소드1 String		
		
		System.out.println("<E> returnType : " + ClassName.genericMethod1("ABCD").getClass().getName()); 		// 제네릭 메소드2 ClassName a	
			System.out.println("<T> returnType : " + ClassName.genericMethod1(a).getClass().getName()); 		// 제네릭 메소드2 Double		
			System.out.println("<T> returnType : " + ClassName.genericMethod1(3.0).getClass().getName());	
	}
}
```

- 결과는 다음과 같다.

![](https://blog.kakaocdn.net/dn/lC9WD/btq8xDRNmH3/Asbt4i2eMEHH2gNCMalvWk/img.png)

- 보다시피 제네릭 메소드는 제네릭 클래스 타입과 별도로 지정된다는 것을 볼 수 있다.
- <> 괄호 안에 타입을 파라미터로 보내 제네릭 타입을 지정해주는 것. 이 것이 바로 제네릭 프로그래밍이다.

  
- **제한된 Generic(제네릭)과 와일드 카드**

  
  
 

지금까지는 제네릭의 가장 일반적인 예시들을 보여주었다. 예로들어 타입을 T라고 하고 외부클래스에서 Integer을 파라미터로 보내면 T 는 Integer가 되고, String을 보내면 T는 String이 된다. 만약 당신이 Student 라는 클래스를 만들었을 때 T 파라미터를 Student로 보내면 T는 Student가 된다. 즉, 제네릭은 참조 타입 모두 될 수 있다.

근데, 만약 특정 범위 내로 좁혀서 제한하고 싶다면 어떻게 해야할까? 

이 때 필요한 것이 바로 extends 와 super, 그리고 ?(물음표)다. ? 는 와일드 카드라고 해서 쉽게 말해 **'알 수 없는 타입'**이라는 의미다.

일단 먼저 예시를 보면서 말해보자면 이용할 때 크게 세 가지 방식이 있다. 바로 super 키워드와 extends 키워드, 마지막으로 ? 하나만 오는 경우다. 코드로 보자면 다음과 같다. 

```
<K extends T>	// T와 T의 자손 타입만 가능 (K는 들어오는 타입으로 지정 됨)<K super T>	// T와 T의 부모(조상) 타입만 가능 (K는 들어오는 타입으로 지정 됨) <? extends T>	// T와 T의 자손 타입만 가능<? super T>	// T와 T의 부모(조상) 타입만 가능<?>		// 모든 타입 가능. <? extends Object>랑 같은 의미
```

보통 이해하기 쉽게 다음과 같이 부른다.

 **extends T : 상한 경계**

**? super T : 하한 경계**

**<?> : 와일드 카드(Wild card)**

이 때 주의해야 할 게 있다. K extends T와 ? extends T는 비슷한 구조지만 차이점이 있다.

'유형 경계를 지정'하는 것은 같으나 경계가 지정되고 **K는 특정 타입으로 지정이 되지만, ?는 타입이 지정되지 않는다는 의미다.**

가장 쉬운 예시로 다음과 같은 예시가 있다.

```
/* * Number와 이를 상속하는 Integer, Short, Double, Long 등의 * 타입이 지정될 수 있으며, 객체 혹은 메소드를 호출 할 경우 K는 * 지정된 타입으로 변환이 된다. */<K extends Number>  /* * Number와 이를 상속하는 Integer, Short, Double, Long 등의 * 타입이 지정될 수 있으며, 객체 혹은 메소드를 호출 할 경우 지정 되는 타입이 없어 * 타입 참조를 할 수는 없다. */<? extends T>	// T와 T의 자손 타입만 가능
```

위와 같은 차이가 있다. 그렇기 때문에 특정 타입의 데이터를 조작하고자 할 경우에는 K 같이 특정 제네릭 인수로 지정을 해주어야 한다. 

일단 위 설명은 잠깐 뒤로하고 extends와 super부터 한 번 하나하나 예를 들어보자.

![](https://blog.kakaocdn.net/dn/bpS4Bp/btqLcTploFp/cTEkLdHVPZW5lnKOvtbS91/img.png)

다음과 같이 서로 다른 클래스들이 상속관계를 갖고 있다고 가정해보자.

**1. <K extends T>, <? extends T>**

이 것은 T 타입을 포함한 자식(자손) 타입만 가능하다는 의미다. 즉, 다음과 같은 경우들이 있다.

```
<T extends B>	// B와 C타입만 올 수 있음<T extends E>	// E타입만 올 수 있음<T extends A>	// A, B, C, D, E 타입이 올 수 있음 <? extends B>	// B와 C타입만 올 수 있음<? extends E>	// E타입만 올 수 있음<? extends A>	// A, B, C, D, E 타입이 올 수 있음
```

주석에 썼듯이 보면 알겠지만, 상한 한계. 즉 extends 뒤에 오는 타입이 최상위 타입으로 한계가 정해지는 것이다.

대표적인 예로는 제네릭 클래스에서 수를 표현하는 클래스만 받고 싶은 경우가 있다. 대표적인 Integer, Long, Byte, Double, Float, Short 같은 래퍼 클래스들은 **Number 클래스를 상속** 받는다.

즉,  Integer, Long, Byte, Double, Float, Short 같은 수를 표현하는 래퍼 클래스만으로 제한하고 싶은 경우 다음과 같이 쓸 수 있다.

```
public class ClassName <K extends Number> { ... }
```

이렇게 특정 타입 및 그 하위 타입만 제한 하고 싶을 경우 쓰면 된다. 좀 더 구체적으로 예로 들자면, 다음과 같다. Integer는 Number 클래스를 상속받는 클래스라 가능하지만, String은 Number클래스와는 완전 별개의 클래스이기 때문에 에러(Bound mismatch)를 띄운다.

```
public class ClassName <K extends Number> { 	... } public class Main {	public static void main(String[] args) { 		ClassName<Double> a1 = new ClassName<Double>();	// OK! 		ClassName<String> a2 = new ClassName<String>();	// error!	}}
```

**2. <K super T>, <? super T>**

이 것은 T 타입의 부모(조상) 타입만 가능하다는 의미다. 즉, 다음과 같은 경우들이 있다.

```
<K super B>	// B와 A타입만 올 수 있음<K super E>	// E, D, A타입만 올 수 있음<K super A>	// A타입만 올 수 있음 <? super B>	// B와 A타입만 올 수 있음<? super E>	// E, D, A타입만 올 수 있음<? super A>	// A타입만 올 수 있음
```

주석에 썼듯이 보면 알겠지만, 하한 한계. 즉 super 뒤에 오는 타입이 최하위 타입으로 한계가 정해지는 것이다.

대표적으로는 해당 객체가 업캐스팅(Up Casting)이 될 필요가 있을 때 사용한다.

예로들어 '과일'이라는 클래스가 있고 이 클래스를 각각 상속받는 '사과'클래스와 '딸기'클래스가 있다고 가정해보자.

이 때 각각의 사과와 딸기는 종류가 다르지만, 둘 다 '과일'로 보고 자료를 조작해야 할 수도 있다. (예로들면 과일 목록을 뽑는다거나 등등..) 그럴 때 '사과'를 '과일'로 캐스팅 해야 하는데, 과일이 상위 타입이므로 업캐스팅을 해야한다. 이럴 때 쓸 수 있는 것이 바로 super라는 것이다.

조금 더 현실성 있는 예제라면 제네릭 타입에 대한 객체비교가 있다.

```
public class ClassName <E extends Comparable<? super E>> { ... }
```

이런 문구를 한 번쯤 보셨을 수 있다. 특히 PriorityQueue(우선순위 큐), TreeSet, TreeMap 같이 값을 정렬하는 클래스 만약 여러분이 특정 제네릭에 대한 자기 참조 비교를 하고싶을 경우 대부분 공통적으로 위와 같은 형식을 취한다.

E extends Comparable 부터 한 번 분석해보자. 

extends는 앞서 말했듯 extends 뒤에오는 타입이 최상위 타입이 되고, 해당 타입과 그에 대한 하위 타입이라고 했다. 그럼 역으로 생각해보면 이렇다. **E 객체는 반드시 Comparable을 구현해야한다는 의미** 아니겠는가?

예제로 보면 이렇다는 것이다.

```
public class SaltClass <E extends Comparable<E>> { ... } public class Student implements Comparable<Student> {	@Override	public int compareTo(Person o) { ... };} public class Main {	public static void main(String[] args) {		SaltClass<Student> a = new SaltClass<Student>();	}}
```

이렇게만 쓴다면 E extends Comparable<E> 까지만 써도 무방하다.

즉, SaltClass의 E 는 Student 이 되어야 하는데, Comparable<Student> 의 하위 타입이어야 하므로 거꾸로 말해 Comparable을 구현해야한다는 의미인 것이다.

그러면 왜 Comparable<E> 가 아닌 <? super E> 일까?

잠깐 설명했지만, super E는 E를 포함한 상위 타입 객체들이 올 수 있다고 했다.

만약에 위의 예제에서 학생보다 더 큰 범주의 클래스인 사람(Person)클래스를 둔다면 어떻게 될까? 한마디로 아래와 같다면?

```
public class SaltClass <E extends Comparable<E>> { ... }	// Error가능성 있음public class SaltClass <E extends Comparable<? super E>> { ... }	// 안전성이 높음 public class Person {...} public class Student extends Person implements Comparable<Person> {	@Override	public int compareTo(Person o) { ... };} public class Main {	public static void main(String[] args) {		SaltClass<Student> a = new SaltClass<Student>();	}}
```

쉽게 말하면 Person을 상속받고 Comparable 구현부인 comparTo에서 Person 타입으로 업캐스팅(Up-Casting) 한다면 어떻게 될까?

만약 <E extends Comparable<E>>라면 SaltClass<Student> a 객체가 타입 파라미터로 Student를 주지만, Comparable에서는 그보다 상위 타입인 Person으로 비교하기 때문에 Comparable<E>의 E인 Student보다 상위 타입 객체이기 때문에 제대로 정렬이 안되거나 에러가 날 수 있다.

그렇기 때문에 E 객체의 상위 타입, 즉 <? super E> 을 해줌으로써 위와같은 불상사를 방지할 수가 있는 것이다.

즉, <E extends Comparable<? super E>> 는 쉽게 말하자면 E 타입 또는 E 타입의 슈퍼클래스가 Comparable을 의무적으로 구현해야한다는 뜻으로 슈퍼클래스타입으로 Up Casting이 발생하더라도 안정성을 보장받을 수 있다.

이 부분은 중요한 것이 이후 필자가 PriorityQueue와 TreeSet 자료구조를 구현할 것인데, 이 부분을 이해하고 있어야 가능하기 때문에 조금은 어렵더라도 미리 언급하고 가려한다.

<E extends Comparable<? super E>> 에 대해 설명이 조금 길었다. 이 긴 내용을 한 마디로 정의하자면 이렇다.

_"E 자기 자신 및 조상 타입과 비교할 수 있는 E"_

**3. <?> (와일드 카드 : Wild Card)**

마지막으로 와일드 카드다. 

이 와일드 카드 <?> 은 <? extends Object> 와 마찬가지라고 했다. Object는 자바에서의 모든 API 및 사용자 클래스의 최상위 타입이다. 한마디로 다음과 같은 의미나 마찬가지다.

```
public class ClassName { ... }public class ClassName extends Object { ... } 
```

우리가 public class ClassName extends Object {} 를 묵시적으로 상속받는 것이나 다름이 없다.

한마디로 <?>은 무엇이냐. 어떤 타입이든 상관 없다는 의미다. 당신이 String을 받던 어떤 타입을 리턴 받던 알빠 아니라는 조금 과격한 얘기..

이는 보통 데이터가 아닌 '기능'의 사용에만 관심이 있는 경우에 <?>로 사용할 수 있다.