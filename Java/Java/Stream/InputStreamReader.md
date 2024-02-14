- inputStreamReader는 byte 대신 char 형태로 읽을 수 있다.
- 따라서 아스키코드가 아닌 문자열로 출력이 가능하며, [[String]] [[객체(Object)]]로 변환할 수도 있게 된다. 
- [[InputStream]]을 인자값으로 받아와서 만들어진다.

- InputStreamReader는 Reader [[클래스(Class)]]를 [[상속(Inheritance)]]한다.
- 즉, [[InputStream]]이 아니다.
- java.io.InputStreamReader라는 class의 package는 java.io.Reader이다.
- java.io.InputStream과에 속하는 것이 아니다.

## 문법

```java
public class InputStreamReader extends Reader
```


- 그렇다면 왜 이름이 InputStreamReader일까
- 바로 매개변수가 InputStream [[인스턴스(Instance)]]이기 때문이다.

```java
InputStreamReader(InputStream in){...}
InputStreamReader(InputStream in, Charset cs){...}
InputStreamReader(InputStream in, CharsetDecoder dec){...}
InputStreamReader(InputStream in, String charsetName){...}
```

- 어떤 경우라도 InputStream 타입의 [[인스턴스(Instance)]]는 필수적으로 들어가야 한다.


```java
public class Sample {
	public static void main(String[] args) throws IOException {
	
	InputStream inputStream = System.in;
	InputStreamReader reader = new InputStreamReader(inputStream);
	
	char[] chars = new char[3];
    reader.read(chars);
    
    System.out.println(chars);
  }
}

// << abc 입력
// >> abc
```

## HTTP 서버의 응답값 불러오기

- 단순히 사용자의 입력만 불러올 수 있는 것이 아니라, HTTP 서버의 응답값도 불러올 수 있다.
- 많은 라이브러리와 객체가 있지만 가장 기본이 되는 [[URLConnection]] [[객체(Object)]]를 사용하는 예시를 살펴보자.

```java
@Component
@RequiredArgsConstructor
@Slf4j
public class UrlComponent1 {
	private final URLConnection urlConnection1 ;
	
	//페이지 출력하기
	public void printPage() throws IOException {
		InputStream inputStream = urlConnection1.getInputStream();
		InputStreamReader reader = new InputStreamReader(inputStream);
		
		char[] chars = new char[4096];
		reader.read(chars);
		
		while((reader.read(chars) != -1)) {
			String s = new String(chars);
			log.info("s = {}", s);
		}
	}
}
```

- urlConnection의 getInputStream() 메서드를 이용해서 InputStream을 불러오고, HTTP Connection을 통해서 [[InputStream]]에 서버의 응답값을 불러오는 예시이다.
- 여기서는 [[HTTP]] 서버 연결 정보나 설정 정보는 생략하였다. 
- 이를 이용하여 네이버에 요청을 보내면 chars에 응답값이 담겨서 log.info를 통해 콘솔에 응답값이 호출된다.

![](https://blog.kakaocdn.net/dn/Aq3NF/btrDPYDmp2z/43kyorKZLq8k495lDTNLW1/img.png)