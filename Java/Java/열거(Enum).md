- enum [[클래스(Class)]]는 열거체(enumeration type)으로 JDK 1.5 이상의 버전에서만 사용이 가능하다.

- [[클래스(Class)]]처럼 보이게 하는 상수이다.
- 서로 관련있는 상수들끼리 모아 상수들을 정의하는 것이다.
- enum 클래스 형을 기반으로 한 클래스형 선언이다.

## enum 클래스의 특징

1. 열거형으로 선언된 순서에 따라 0부터 index 값을 가진다.(순차적으로 증가한다.)
2. enum 열거형으로 지정된 상수들은 모두 대문자로 선언한다. 
3. 열거형 변수들을 선언한 후 마지막에 세미콜론(;)을 찍지 않는다.
4. 상수와 특정 값을 연결시킬경우 마지막에 세미콜론(;)을 붙여줘야한다.

## 문법

- 열거체 정의는 enum 클래스용인 java파일로 선언, 타 클래스 내부에 선언, 클래스 외부에 선언 등으로 이용하고는 하는데, 일반적으로 열거체 정의용 java class 파일을 따로 만들어 사용하고는 한다.

```java
// 정의
enum 열거체 이름 {상수1, 상수2, ...}  

// 코드로 작성
enum Company {SK, LG, KT, SAMSUNG, APPLE}  

// 호출하는 방식
Company.APPLE
```

## enum 메서드

- 여기서 주로 사용되는 메서드는 values(), ordinal(), valueOf() 정도이다.

## 예시 

- 밑의 코드는 열거체 정의(Company.java)하는 방법이다.

```java
public enum Company {

    SK("에스케이"),
    LG("엘쥐"),
    KT("케이티"),
    SAMSUNG("삼성"),
    APPLE("애플");
    
    private final String value;
    
    Company(String value){
        this.value = value;
            
    }
    
    public String getValue(){
        return value;
    }

}
```

- 상수와 특정값을 연결시켜놓은건데 특정값을 연결시키려면 해당 값들을 리턴할 수 있는 함수가 선언되어있어야한다.
- enum 상수와 연결된 값을 가져온다.

```java
public class TestEnum {
    public static void main(String[] args) {
    
        for(Company type : Company.values()){
            System.out.println(type.getValue()); // 에스케이, 엘쥐, 케이티, 삼성, 애플
        }
        
        System.out.println(Company.APPLE.getValue()); // 애플   
    }
}
```

- enum [[클래스(Class)]] [[메서드(Method)]] 사용하는 예시이다.(ft. values(), ordinal())

```java
public class TestEnum {

    public static void main(String[] args) {

        for(Company type : Company.values()){
            System.out.println(type); // SK, LG, KT, SAMSUNG, APPLE
        }

        System.out.println(Company.APPLE.ordinal()); // 4

    }
}
```

- values()는 enum에 선언된 상수를 전부 가져온다.(예시 출력을 위해 for문에 사용했다.)
- ordinal()는 해당 상수의 index값을 출력한다.

 - switch문에 사용하는 예시이다.

```java
public class TestEnum {
    enum Company { SK, LG, KT, SAMSUNG, APPLE }
    
    public static void main(String[] args) {
        int test = 1;
        
        if(test == 1){
            switchFt(Company.SAMSUNG);
    }...
    
    public static void switchFt(Company company){
    
        switch(company){
	        case SK :
	            System.out.println("SK 입니다.");
	            break;
	        case LG :
	            System.out.println("LG 입니다.");
	            break;
	        case KT :
	            System.out.println("KT 입니다.");
	            break;
	        case SAMSUNG :
	            System.out.println("SAMSUNG 입니다.");
	            break;
	        case APPLE :
	            System.out.println("APPLE 입니다.");
	            break;
        }
    }   
}
```
