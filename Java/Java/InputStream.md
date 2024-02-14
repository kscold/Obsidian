- InputStream은 JDK 1.0에서 도입되었다.
- java.io 패키지에 속해 있다.

- InputStream은 1 byte의 int형의 아스키 코드값으로 값을 저장한다.
- InputStream [[객체(Object)]]를 [[상속(Inheritance)]]받는 System.in의 read() 메서드를 통해서 값을 입력받을 수 있다.

- 아래 코드를 main 메서드에 입력 후, 콘솔창에 a라는 문자를 입력하고 Enter를 누르면 아스키코드 값인 97이 출력되는 것을 확인할 수 있다.

```java
public class Sample {
    public static void main(String[] args) throws IOException {
        InputStream in = System.in;
        
        int a;
        a = in.read();
        
        System.out.println(a);
    }
}
// << a
// >> a = 97
```

- 만약 콘솔창에서 abc를 입력하더라도 맨 앞의 "a"에 해당하는 아스키 코드값 97만 출력된다. 
- 왜냐하면 바이트코드를 저장할 수 있는 스트림 데이터를 int로 정의했기 때문이다. 
- 만약 아래와 같이 byte 배열로 지정한다면, 그 길이만큼 바이트를 받을 수 있다.

- 만약 아래처럼 길이가 3 바이트인 byte array를 지정하고 read 메서드를 실행하면, abc, 즉 3바이트까지 읽어들일 수 있는 것을 확인할 수 있다.

```java
public class Sample {

  public static void main(String[] args) throws IOException {
  
    InputStream inputStream = System.in;
    
    byte[] bytes = new byte[3];
    inputStream.read(bytes);
    
    System.out.println(bytes[0]);
    System.out.println(bytes[1]);
    System.out.println(bytes[2]);
    
  }
}

// << abc
// >> 97
// >> 98
// >> 99
```
