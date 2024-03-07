- CORS(Cross-Origin Resource Sharing Policy) 보안 정책이 발생하는 이유를 간단히 이야기 하자면, SOP 때문이다.

## SOP

- SOP는 Same Origin Policy로, 동일한 출처의 Origin만 리소스(데이터)를 공유할 수 있는 것이다.

- 만약 동일한 출처가 아니라면 OPTIONS 를 이용한 Preflight 를 이용해서 **여러 검증**을 거치게 되는데, 이 검증이 바로 CORS인 것이다.

## CORS 플로우

- Front Back 구조에서 SOP를 위반하고, 적절한 보안 인증을 받기 위해서 브라우저는 다음과 같은 과정을 거치게 된다.

![](https://blog.kakaocdn.net/dn/x9GpQ/btrb6sFd8WS/lPduMl313h54tFO3mslwU0/img.png)

1. GET 요청인지 POST 요청인지 파악한다.
2. Content-Type 과 Custom HTTP Header 를 파악한다.
3. OPTIONS 요청을 통해서 서버가 적절한 Access-Control-* 를 가졌는지 확인한다.
4. 만약 적절한 Access-Control 을 가졌다면 실제 XHR을 트리거한다.
5. 적절하지 못한 Access-Control 를 가졌다면 Error 를 발생시킨다.

- 이 플로우에서 적절한 대처를 하지 못한다면 발생하는 것이 맨 위에서 본 CORS 에러인 것이다.

## Spring Boot Application 에서 CORS 해결하기

- Preflight 요청에서 적절한 Access-Control 을 확인하지 못하면 발생하게 된다.

- 이런 Preflight 상황에서 적절한 Access-Control 을 위한 해결 방법이 3가지가 존재한다.
	1. `CorsFilter` 로 직접 response에 header 를 넣어준다.
	2. [[컨트롤러(Controller)]]에서 `@CrossOrigin` [[어노테이션(Annotation)]]추가한다.
	3. [[WebMvcConfigurer]]를 이용해서 처리한다.


## [[WebMvcConfigurer]] 설정

- WebMvcConfigurer를 상속한 Config 설정을 만들고 addCorsMappings [[메서드(Method)]]를 [[오버라이딩(Overriding)]]하여 CORS 설정을 한다.

```java
public class SomeConfig implements WebMvcConfigurer {

	@Override  
	public void addCorsMappings(CorsRegistry registry) {  
	    // CORS 설정을 하기 위해 메서드 추가  
	    registry.addMapping("/**") // 모든 패스에서 설정  
	            .maxAge(500) // 최대 시간 설정  
	            .allowedMethods("GET","POST","PUT","DELETE","HEAD","OPTION") 
	            // 허용할 메서드 설정, OPTION으로 preflight 허용을 설정함  
	            .allowedOrigins("*"); // 허락할 리소스를 모두로 설정  
	}
}
```