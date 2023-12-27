- [[@RequestMapping]]에서 @RequestMapping(value = "/경로", method = RequestMethod.GET)을 합쳐서 만든 어노테이션

#### 형식
```java
@GetMapping(“/경로”)
```

@GetMapping(“예를 들어 hello")는 HTTP GET 요청 중 URL 경로가 "hello"인 것들을 해당 [[메서드(Method)]]와 연결(bind)시켜준다.