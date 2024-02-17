- 자바의 자료형은 크게 [[원시 타입(Primitive Type)]]과 [[참조 타입(Reference Type)]]으로 나누어진다.

- 대표적으로 원시 타입은 char, int, float, double, boolean 등이 있고 참조 타입은 class, interface 등이 있는데 프로그래밍을 하다 보면 기본 타입의 데이터를 객체로 표현해야 하는 경우가 종종 있다. 

- 이럴 때에 [[원시 타입(Primitive Type)]]을 객체로 다루기 위해서 사용하는 클래스들을 래퍼 클래스(wrapper class)라고 한다.

- 자바는 모든 [[원시 타입(Primitive Type)]]은 값을 갖는 [[객체(Object)]]를 생성할 수 있다.

- 이런 객체를 Wrapper(포장) [[객체(Object)]] 라고도 하는데 그 이유는 원시 타입의 값을 내부에 두고 포장하기 때문이다.

- [[Wrapper]] [[클래스(Class)]]로 감싸고 있는 [[원시 타입(Primitive Type)]] 값은 외부에서 변경할 수 없다.
- 만약 값을 변경하고 싶다면 새로운 포장 객체를 만들어야 한다.


## Wrapper 클래스의 종류

| 원시 타입(primitive type) | 래퍼 클래스(wrapper class) |
| ---- | ---- |
| byte | Byte |
| char | Character |
| int | Integer |
| float | Float |
| double | Double |
| boolean | Boolean |
| long | Long |
| short | Short |

- 래퍼 클래스는 java.lang 패키지에 포함되어 있는데, 다음과 같이 기본 타입에 대응되는 래퍼 클래스들이 있다.
- char타입과 int타입이 각각 Character와 Integer의 래퍼 클래스를 가지고 있고 나머지는 기본 타입의 첫 글자를 대문자로 바꾼 이름을 가지고 있다.

## 래퍼 클래스 구조도

![래퍼클래스 구조도](https://blog.kakaocdn.net/dn/bvzp79/btqEbacB01v/QQjO7cSc9tTvKJkyzFsK90/img.png)

- 위의 계층 구조에서 볼 수 있듯 모든 래퍼 클래스의 부모는 Object이고 내부적으로 숫자를 다루는 래퍼클래스의 부모 클래스는 Number 클래스이다.
- 모든 래퍼 클래스는 최종 클래스로 정의된다.

##  박싱(Boxing)과 언박싱(UnBoxing) 

![박싱 언박싱](https://blog.kakaocdn.net/dn/38MsL/btqEbRcxIfZ/fOMbL4b3wCzqeO1aKKbFZ0/img.png)

- 기본 타입의 값을 포장 객체로 만드는 과정을 박싱이라고 하고 반대로 포장객체에서 기본타입의 값을 얻어내는 과정을 언박싱이라고 한다.

```java
public class Wrapper_Ex {
    public static void main(String[] args)  {
        Integer num = new Integer(17); // 박싱
        int n = num.intValue(); //언박싱
        System.out.println(n);
    }
}
```

## 자동 박싱(AutoBoxing)과 자동 언박싱(AutoUnBoxing)

- 원시 타입 값을 직접 박싱, 언박싱하지 않아도 자동적으로 박싱과 언박싱이 일어나는 경우가 있다. 
- 자동 박싱의 포장 클래스 타입에 기본값이 대입될 경우에 발생한다.

- 예를 들어 int 타입의 값을 Integer 클래스 변수에 대입하면 자동 박싱이 일어나 힙 영역에 Integer 객체가 생성된다.

```java
public class Wrapper_Ex {
    public static void main(String[] args)  {
        Integer num = 17; // 자동 박싱
        int n = num; //자동 언박싱
        System.out.println(n);
    }
}
```

##  문자열을 기본 타입 값으로 변환

- 밑의 코드는 문자열을 기본 타입 값으로 변환하는 예시이다.

```java
public class Wrapper_Ex {
	public static void main(String[] args)  {
        String str = "10";
        String str2 = "10.5";
        String str3 = "true";
        
        byte b = Byte.parseByte(str);
        int i = Integer.parseInt(str);
        short s = Short.parseShort(str);
        long l = Long.parseLong(str);
        float f = Float.parseFloat(str2);
        double d = Double.parseDouble(str2);
        boolean bool = Boolean.parseBoolean(str3);
		
        System.out.println("문자열 byte값 변환 : " + b);
        System.out.println("문자열 int값 변환 : " + i);
        System.out.println("문자열 short값 변환 : " + s);
        System.out.println("문자열 long값 변환 : " + l);
        System.out.println("문자열 float값 변환 : " + f);
        System.out.println("문자열 double값 변환 : " + d);
        System.out.println("문자열 boolean값 변환 : " + bool);
    }
}
```

![문자열을 기본타입으로](https://blog.kakaocdn.net/dn/b7SA9k/btqEam5KAmB/bkYwsl6gZGncA8iLIHCpW1/img.png)

- 래퍼 클래스의 주요 용도는 기본 타입의 값을 박싱 해서 포장 객-체로 만드는 것이지만, 문자열을 기본 타입 값으로 변환할 때에도 사용된다.

- 대부분의 래퍼 클래스에는 parse + 기본 타입명으로 되어있는 [[정적 메서드(Static Method)]]가 있다.
- 이 메서드는 문자열을 매개 값으로 받아 기본 타입 값으로 변환한다.

## 값 비교

```java
public class Wrapper_Ex {
    public static void main(String[] args)  {
        Integer num = new Integer(10); // 래퍼 클래스1
        Integer num2 = new Integer(10); // 래퍼 클래스2
        int i = 10; //기본타입
		 
        System.out.println("래퍼클래스 == 기본타입 : " + (num == i)); // true
        System.out.println("래퍼클래스.equals(기본타입) : " + num.equals(i)); // true
        System.out.println("래퍼클래스 == 래퍼클래스 : " + (num == num2)); // false
        System.out.println("래퍼클래스.equals(래퍼클래스) : " + num.equals(num2)); // true
    }
}
```

![값 비교](https://blog.kakaocdn.net/dn/F6iwX/btqEalTlXQb/zfZW8M9uWp4vhGLN6dgaZK/img.png)

- 래퍼 객체는 내부의 값을 비교하기 위해 == 연산자를 사용할 수 없다. 
- 이 연산자는 내부의 값을 비교하는 것이 아니라 래퍼 객체의 참조 주소를 비교하기 때문이다.

- 비교 대상인 래퍼는 객체이므로 서로의 참조 주소가 다릅니다. 객체끼리의 비교를 하려면 내부의 값만 얻어 비교해야 하기에 [[equals()]]를 사용해야 한다.

- Wrapper 클래스와 원시 타입과의 비교는 == 연산과 equals 연산 모두 가능하다.
- 그 이유는 컴파일러가 자동으로 오토박싱과 언박싱을 해주기 때문이다.