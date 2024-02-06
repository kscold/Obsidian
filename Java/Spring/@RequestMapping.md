
- 우리는 특정 uri로 요청을 보내면 [[컨트롤러(Controller)]]에서 어떠한 방식으로 처리할지 정의를 한다.

- 이때 들어온 요청을 특정 [[메서드(Method)]]와 매핑하기 위해 사용하는 것이 @RequestMapping이다.

- @RequestMapping에서 가장 많이사용하는 부분은 value와 method이다.(추가적인 속성이 더 존재한다.)

- 여기서 파생된 [[@GetMapping]]과 [[@PostMapping]]이 존재한다.


#### 형식
```java
@RequestMapping(value = "/hello", method = RequestMethod.GET)
```

- value는 요청받을 url을 설정하게 된다.

- method(GET, POST, PUT, DELETE)는 어떤 요청으로 받을지 정의하게 된다.