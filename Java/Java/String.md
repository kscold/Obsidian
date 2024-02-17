- 자바에서 문자열을 나타내는 자료형은 String이다. 앞의 문자열을 자바에서 표현해 보자.
- String은 [[원시 타입(Primitive Type)]]에는 포함되지 않는다.

## 문법

```java
String a = "Happy Java";  // 문자열의 앞과 뒤는 쌍따옴표("")로 감싸야 한다.
String b = "a";
String c = "123";
```

- 다음과 같이 [[new]] 키워드를 사용하여 표현할 수도 있다.

```java
CopyString a = new String("Happy Java");
String b = new String("a");
String c = new String("123");
```

- 가급적이면 첫 번째 방식, 즉 리터럴(literal)([[원시 타입(Primitive Type)]]) 표기 방식을 사용하는 것이 좋다. 
- 왜냐하면 리터럴 표기 방식은 가독성이 좋고 컴파일할 때 최적화에 도움을 주기 때문이다.
## 메서드

- String 자료형의 내장 [[메서드(Method)]] 중에서 자주 사용하는 것을 알아보자. 
- String 자료형의 내장 [[메서드(Method)]]는 문자열 객체에 속한 함수라 할 수 있다. 
- 문자열 합치기, 분할, 대소문자 변환 등의 문자열을 다양하게 활용할 수 있도록 도와주는 역할을 한다.

### [[equals()]]

- equals 메서드는 문자열 2개가 같은지를 비교하여 결괏값을 리턴한다.

```java
String a = "hello";
String b = "java";
String c = "hello";
System.out.println(a.equals(b)); // false 출력
System.out.println(a.equals(c)); // true 출력
```

```no-highlight
false
true
```

- 문자열 a와 문자열 b에는 hello와 java가 각각 저장되어 있으므로 값이 서로 같지 않다.
- 따라서 equals 메서드를 호출하면 false를 리턴한다. 그러나 문자열 a와 문자열 c는 hello와 hello로 값이 서로 같으므로 true를 리턴한다.

- 이처럼 문자열의 값을 비교할 때는 반드시 equals를 사용해야 한다. 
- 만약 equals 대신 `==` 연산자를 사용한다면 다음과 같은 결과가 발생한다.

```java
String a = "hello";
String b = new String("hello");
System.out.println(a.equals(b));  // true
System.out.println(a == b);  // false
```

```no-highlight
true
false
```

- 문자열 a와 b는 모두 hello로 값이 같지만 equals를 호출하면 true를, `==` 연산자를 사용하면 false를 리턴한다.
- a와 b는 값은 같지만 서로 다른 객체이기 때문이다.
- `==`은 2개의 자료형이 같은 객체인지를 판별할 때 사용하는 연산자이므로 false를 리턴한다.

### indexOf

indexOf는 문자열에서 특정 문자열이 시작되는 위치(인덱스값)를 리턴한다. 문자열 a에서 Java가 시작되는 위치를 알고 싶다면 indexOf를 사용하여 다음처럼 위치를 리턴받을 수 있다.

```java
String a = "Hello Java";
System.out.println(a.indexOf("Java"));  // 6 출력
```

```no-highlight
6
```

문자열 Hello Java에서 특정 문자열인 Java는 일곱 번째 문자인 J부터 시작된다. 이때 결괏값이 7이 아닌 6으로 나온 이유는 자바에서는 숫자를 0부터 세기 때문이다.

### contains

contains 메서드는 문자열에서 특정 문자열이 포함되어 있는지 여부를 리턴한다.

```java
String a = "Hello Java";
System.out.println(a.contains("Java"));  // true 출력
```

```no-highlight
true
```

문자열 a는 Java라는 문자열을 포함하고 있어서 true를 리턴한다.

### charAt

charAt 메서드는 문자열에서 특정 위치의 문자를 리턴한다. Hello Java 문자열에서 J는 여섯번째 인덱스에 위치한 문자이다. 인덱스 6으로 문자 J를 리턴받으려면 다음과 같이 charAt을 사용한다.

```java
String a = "Hello Java";
System.out.println(a.charAt(6));  // "J" 출력
```

```no-highlight
J
```

### replaceAll

replaceAll 메서드는 문자열에서 특정 문자열을 다른 문자열로 바꿀 때 사용한다. 다음 예에서는 Hello Java 문자열에서 Java를 World로 바꾸었다.

```java
String a = "Hello Java";
System.out.println(a.replaceAll("Java", "World"));  // Hello World 출력
```

```no-highlight
Hello World
```

### substring

substring 메서드는 문자열에서 특정 문자열을 뽑아낼 때 사용한다.

```java
String a = "Hello Java";
System.out.println(a.substring(0, 4));  // Hell 출력
```

```no-highlight
Hell
```

위처럼 substring(시작 위치, 끝 위치)와 같이 코드를 작성하면 문자열의 시작 위치에서 끝 위치까지의 문자를 뽑아내게 된다. 단, 끝 위치의 문자는 포함이 안 된다는 점에 주의하자. 이것은 다음과 같은 수학의 식과 비슷하다.

```no-highlight
시작위치 <= a < 끝위치
```

### toUpperCase

toUpperCase 메서드는 문자열을 모두 대문자로 변경할 때 사용한다.

```java
String a = "Hello Java";
System.out.println(a.toUpperCase());  // HELLO JAVA 출력
```

```no-highlight
HELLO JAVA
```

> 문자열을 모두 소문자로 변경할 때는 toLowerCase를 사용한다.

### split

split 메서드는 문자열을 특정한 구분자로 나누어 문자열 배열로 리턴한다.

```java
String a = "a:b:c:d";
String[] result = a.split(":");  // result는 {"a", "b", "c", "d"}
```

이번 예처럼 a:b:c:d라는 문자열을 :(콜론)으로 나누어 `{"a", "b", "c", "d"}` 문자열 배열을 만들 수 있다.

> 배열은 03-6절에서 자세히 알아보자.

