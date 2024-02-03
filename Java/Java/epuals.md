- 자바 문자열의 내용 비교를 할 때 사용한다.
- equals() [[메서드(Method)]]이다.

## 비교 연산자와 equals() 메서드의 차이

### == (비교 연산자)

- 주소의 값을 비교한다.([[참조 타입(Reference Type)]], Call By Reference)
- 주소의 값이란 실제 내용의 값이 아닌 자료의 위치의 값이다.
### equals() 메서드

- equals() [[메서드(Method)]]는 [[객체(Object)]]끼리 내용 비교한다.([[원시 타입(Primitive Type)]], Call By value)

```java
package joon;

public class codeTest {
    public static void main(String[] args) throws Exception{
    
        String str1 = "abc";
        String str2 = str1;
        String str3 = new String("abc");
        
        // == 연산자는 주소를 비교
        System.out.println(str1 == str2); // true
        // str2 에 st1 값을 넣었으므로 주소를 같이 공유
        
        System.out.println(str1 == str3); // false
        // str1 과 str3는 각각 생성 되었으므로 주소가 다름
        
        // equals() 는 내용을 비교함
        System.out.println(str1.equals(str2)); // ture
        System.out.println(str1.equals(str3)); // true
        // 내용을 비교하기떄문에 abc 내용이 같으므로 true 가 반환
    }
}
```

- 따라서 문자열을 비교할때에는 == 보다는 equals를 사용는 것이 좋다.

## equals로 null 체크 하는 방법

```java
package joon;

public class codeTest {
    public static void main(String[] args) throws Exception{
    
        String str1 = "2";
        String str2 = "";
        
        if(!"".equals(str1)){
            System.out.println("공백이 아닙니다.");
        }
        
        if("".equals(str2)){
            System.out.println("공백입니다.");
        }
    }
}
```