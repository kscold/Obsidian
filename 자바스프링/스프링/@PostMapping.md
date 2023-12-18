
- [[@RequestMapping]]에서 @RequestMapping(value = "/경로", method = RequestMethod.POST)을 합쳐서 만든 어노테이션d이다.

#### 형식
```java
@PostMapping(“/경로”)
```

@PostMapping(“예를 들어 hello")는 HTTP POST 요청 중 URL 경로가 "hello"인 것들을 해당 [[메서드(Method)]]와 연결(bind)시켜준다.