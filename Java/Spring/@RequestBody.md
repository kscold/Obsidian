- 스프링에서 [[비동기(Asynchronous)]] 요청(Request) 처리를 하는 경우 @ResponseBody를 사용한다.

- 이 [[어노테이션(Annotation)]]이 붙은 매개변수에는 [[HTTP]] 요청(Request)의 본문(body)이 그대로 전달된다. 

- 일반적인 GET/POST의 요청 파라미터라면 @RequestBody를 사용할 일이 없을 것이다.
- 반면에 xml이나 json기반의 메시지를 사용하는 요청의 경우에 이 방법이 매우 유용하다.

- @RequestBody를 통해서 자바 [[객체(Object)]]로 conversion을 하는데, 이때 HttpMessageConverter를 사용한다. 
- @ResponseBody 가 붙은 파라미터에는 [[HTTP]] 요청의 분문(body) 부분이 그대로 전달된다.
- RequestMappingHandlerAdpter 에는 HttpMessageConverter 타입의 메세지 변환기가 여러개 등록되어 있다.


```java
@RequestMapping(value = "/ajaxTest.do")
public String ajaxTest(@RequestBody UserVO getUserVO) throws Exception {
	// 매개변수에 xml이나 json기반의 메시지를 사용하는 요청의 경우 유용
	System.out.println(getUserVO.getId());
	
	return "test/login.tiles";
}
```