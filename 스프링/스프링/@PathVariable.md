
- REST API에서 URI에 변수가 들어가는걸 실무에서 많이 볼 수 있다.

예를 들면, 아래 URI에서 밑줄 친 부분이 @PathVariable로 처리해줄 수 있는 부분이다.

http://localhost:8080/api/user/1234
https://music.bugs.co.kr/album/4062464

### 문법
- Controller에서 아래와 같이 작성하면 간단하게 사용 가능하다.

1. @GetMapping(PostMapping, PutMapping 등 다 상관없이 사용한다.)에 {변수명} 경로를 받는다.
2. [[메서드(Method)]] 정의에서 위에 쓴 변수명을 그대로 @PathVariable("변수명") 사용한다.
3. (Optional) 매개변수명은 아무거나 상관없다.(아래에서 String name도 가능하고, String employName도 가능하다.)

```java
@RestControllerpublic 
class MemberController { // 기본

	@GetMapping("/member/{name}")    
	public String findByName(@PathVariable("name") String name ) { 
		return "Name: " + name;    
	}

	// 여러 개
	@GetMapping("/member/{id}/{name}")	
	public String findByNameAndId(@PathVariable("id") String id, @PathVariable("name") String name) {    	
		return "ID: " + id + ", name: " + name;    
	}   
	 
}
```

