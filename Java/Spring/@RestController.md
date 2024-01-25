- REST API용으로 사용할 [[Controller]]라는 것을 명시해주는 [[어노테이션(Annotation)]]이다.

- @RestController는 [[@Controller]]에 [[@ResponseBody]]가 추가된 것이다.

- 당연하게도 RestController의 주용도는 [[JSON(Java Script Object Notation)]] 형태로 [[객체(Object)]] 데이터를 반환하는 것이다.
- 최근에 데이터를 응답으로 제공하는 REST API를 개발할 때 주로 사용하며 객체를 [[ResponseEntity]]로 감싸서 반환한다.
- 이러한 이유로 동작 과정 역시 @Controller에 @ReponseBody를 붙인 것과 완벽히 동일하다.

- [[@Controller]]와 큰 차이점은 [[@Controller]]는 [[View]] 페이지인 html 값으로 반환한다는 것이고 @RestController는 [[JSON(Java Script Object Notation)]]이나 텍스트 같은 데이터를 반환한다.

## @RestController 동작과정

![](https://blog.kakaocdn.net/dn/bozaJn/btsteSlYKCB/5WrQAZZDVpLjKSwVPw4cK1/img.png)

1. Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
2. DispatcherServlet이 요청을 처리할 대상을 찾는다.
3. HandlerAdapter을 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후에 객체를 반환한다.
5. 반환되는 객체는 Json으로 Serialize되어 사용자에게 반환된다.

