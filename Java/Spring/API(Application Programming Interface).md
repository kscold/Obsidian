- API란 Application Programming Interface의 약자로, 어떤 서버의 특정한 부분에 접속해서 그 안에 있는 데이터와 서비스를 이용할 수 있게 해주는 소프트웨어 도구이다.

- [[JSON(Java Script Object Notation)]]을 뱉는 방식으로 [[@ResponseBody]] [[어노테이션(Annotation)]]을 선행적으로 이용해 필요한 값으로만 json 형식 파일을 만든다.

## 예시

 - 먼저 Hello 클래스를 정의한다.
 
```java
// Hello.class
public class Hello { // 정적메서드로 객체를 생성할 수 있음
	private String name;
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
}
```

```java
@GetMapping("hello-api") // hello-api라는 라우팅을 생성
@ResponseBody // 로직이 적용된 데이터 값만 반환
public Hello helloApi(@RequestParam("name") String name){ 
	// url인 hello-api?name=사용자입력 을 사용할 수 있는 함수 생성
	
	Hello hello = new Hello(); 
	// 함수를 하나의 새로운 객체 new Hello()로 선언하여 인스턴스화
	
	hello.setName(name); // 이름을 설정
	return hello;
}
```
