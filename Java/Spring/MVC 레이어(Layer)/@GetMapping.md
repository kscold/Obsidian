- [[@RequestMapping]]에서 @RequestMapping(value = "/경로", method = RequestMethod.GET)을 합쳐서 만든 어노테이션

- @GetMapping("경로/{변수}") 처럼 {}를 사용하여 변수를 넣을 수 있다.

#### 형식
```java
@GetMapping(“/경로”)
```

@GetMapping(“예를 들어 hello")는 HTTP GET 요청 중 URL 경로가 "hello"인 것들을 해당 [[메서드(Method)]]와 연결(bind)시켜준다.


### 예시
- {} 변수에 값이 매핑될 때에는 [[@PathVariable]]

```java
@GetMapping("/articles/{id}")  
public String show(@PathVariable Long id) { // 매개변수로 id 받아 오기  
    log.info("id = " + id); // id를 잘 받았는지 확인하는 로그 찍기  
    return "";  
}
```