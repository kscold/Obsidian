- [[변수(Variable)]]의 접근가능 범위를 스코프라고 한다.

- 밑의 코드는 [[지역 변수(local variable)]]의 예시이다.

```java
package scope;  
  
public class Scope1 {  
    public static void main(String[] args) {  
        int m = 10; // m 생존 시작  
        if (true) {  
            int x = 20; // x 생존 시작  
            System.out.println("if m = " + m);  
            System.out.println("if x = " + x);  
        } // x 생존 종료  
  
        // System.out.println("if x = " + x); // 오류 변수 x에 접근 불가  
        System.out.println("if m = " + m);  
    } // m 생존 종료  
}
```

## 스코프의 존재 이유

- 필요한 부분에서만 사용해서 자원을 낭비하지 않도록 만들기 위해 스코프 개념을 사용한다.

```java
package scope;  
  
public class Scope3_1 {  

    public static void main(String[] args) {  
        int m = 10;
        
        if (m>0){  
            int temp = 0; // 지역변수 범위를 잘 정했으니 좋은코드임
            System.out.println("temp = " + temp);  
        }  
        System.out.println("m = " + m);  
    }  
}
```

## while 문과 for 문의 스코프 차이

- while 문의 경우 i의 스코프가 for 문보다 훨씬 더 길 수 밖에 없다.
- 따라서 공간복잡도 측면에서 for문이 더 유리하다.

```java
public class While {  

    public static void main(String[] args) {  
        int sum = 0;  
        int endNum = 3;  
        int i = 0; // i 생존 시작
  
        while (i < endNum) {  
            sum = sum + i;  
            System.out.println("i = " + i + " sum = " + sum);  
            i++;  
        }  
    }  
} // i 생존 종료
```

```java
public class For {  

    public static void main(String[] args) {  
        int sum = 0;  
        int endNum = 3;  
  
        for (int i = 0; i < endNum; i++) {  // i 생존 시작
            sum = sum + i;  
            System.out.println("i = " + i + " sum = " + sum);  
        } // i 생존 종료
  
    }  
}
```