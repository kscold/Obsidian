- 스프링에서 [[비동기(Asynchronous)]] 응답(Response) 처리를 하는 경우 @ResponseBody를 사용한다.
- 자바 [[객체(Object)]]를 [[HTTP]] 요청의 body(본문) 내용으로 매핑하여 클라이언트로 전송한다.

- @ResponseBody가 붙은 매개변수가 있으면 [[HTTP]]요청의 미디어타입과 매개변수의 타입을 먼저 확인한다.
- dispatcher-servlet.xml 의 `<annotation-drvien>` 태그 내에 선언하는 `<message-converter>` 에서 확인한다.

![](https://blog.kakaocdn.net/dn/Ljh4L/btq4dcqj9lb/kNckzEIKwF6tXQ4z4ItSC0/img.png)

- 메세지 변환기 중에서 해당 미디어타입과 파라미터 타입을 처리할 수 있다면, HTTP요청의 본문 부분을 통째로 변환해서 지정된 [[메서드(Method)]] 매개변수로 전달해준다.

```java
@ResponseBody
@RequestMapping(value = "/ajaxTest.do")
public UserVO ajaxTest() throws Exception {

  UserVO userVO = new UserVO();
  userVO.setId("테스트");

  return userVO;
}
```

- 즉, @Responsebody [[어노테이션(Annotation)]]을 사용하면 [[HTTP]] 요청 body를 자바 [[객체(Object)]]로 전달받을 수 있다.