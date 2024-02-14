- BufferedReader 클래스는 버퍼를 이용하는 대표적인 I/O(Input/Output) [[클래스(Class)]]이다
- 입력된 데이터를 바로 전달하는 것이 아닌, 버퍼에 저장해두었다가 전달하는 방법이다.

- 보통 자바에서 입력은 Scanner 클래스를 배워 사용한다.
- 두 클래스의 차이점은, Scanner 클래스는 Space, Enter 모두 경계로 입력값을 인식하고, BufferedReader 클래스는 Enter만을 경계로 입력값을 인식한다.

- 그래서 가공하는 작업이 추가로 필요하다.(예를 들어 구분하여 나누기 작업(split)이다.)
- 데이터의 양이 적을 때는 차이가 미미하겠지만 양이 많을 경우에 하나하나씩 전달하지 않고 버퍼에 한 번에 모아서 전달하는 BufferedReader클래스가 속도면에서 빠르고 효율적이다.

![](https://blog.kakaocdn.net/dn/mMFNq/btqEd7Nv1Nt/I9hOnkwIpnaJS7pYcta6NK/img.png)

## BufferedReader의 종류

- Stream으로 끝나는 클래스 : 바이트 단위로 입출력을 수행하는 클래스이다.
- Reader / Writer로 끝나는 클래스 : 캐릭터 단위로 입출력을 수행하는 클래스이다.
- File로 시작하는 클래스 : 하드디스크의 파일을 사용하는 클래스이다.
- Data로 시작하는 클래스 : 자바의 원시 자료형을 출력하기 위한 클래스이다.
- Buffered로 시작하는 클래스 : 시스템의 버퍼를 사용하는 클래스이다.

## 예시

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader; 

public static void main(String[] args) throws IOException {        
	//선언 
	BufferedReader bf = new BufferedReader(new InputStreamReader(System.in)); // 파일에서 입력받을 경우에는 
	
	new BufferedReader(new FileReader("ex.java")); //라인단위로 입력받기(Enter를 경계로)
	String str = bf.readLine(); // 정수형 입력이라면, 형변환 필수!
	int i = Integer.parseInt(bf.readLine()); // Space를 경계로 끊어야 할 때
	String arr[] = str.split(" "); // 또는 StringTokenizer 클래스 이용)
}
```

## 메서드

### int read()

- [[스트림(Stream)]]으로부터 한 문자를 읽어서 int형으로 반환한다. 
- '3' -> (int)'3' -> 51 과정처럼 아스키코드로 반환된다.

### int read(char[] buf)

- 스트림으로부터 buf의 크기만큼 문자를 읽는다.
- 문자 수를 반환한다.

### int read(char[] buf, int offset, int length)

- 스트림으로부터 buf의 offset 위치에서부터 length 길이만큼 문자를 읽어들인다.

### String readLine() 

- 스트림으로부터 한 줄을 읽어 문자열로 리턴한다.

### void mark() 

- 스트림의 현재위치를 마킹, 차후 reset() 이용하여 마킹위치부터 시작한다.

### void reset()

- 마킹이 있으면 그 위치부터, 없으면 처음부터 다시 시작한다.

### long skip(int n)

- n개의 문자를 건너 뛴다.

