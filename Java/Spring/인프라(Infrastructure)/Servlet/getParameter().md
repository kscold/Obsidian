- [[HttpServletRequest]]에 있는 [[클래스(Class)]]로 getParameter()는 String 형태의 쿼리 스트링을 반환한다.

## 문법

```java
String getParameter(name) // 쿼리스트링을 반환
```
 

## 예시

```java
@RequestMapping("/test") 
public String test(HttpServletRequest req) {

	String userId = req.getParameter("userId"); // 예시 /test?userId=123
	return "test"; // test jsp나 mustache 반환
}
```

