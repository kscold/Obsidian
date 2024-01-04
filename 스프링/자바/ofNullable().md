- 자바에서 제공하는 객체를 [[Optional]] 객체로 감싸기 위해서는 Optional 에서 제공하는 [[of()]] 와 ofNullable [[메서드(Method)]]를 사용한다.
- ofNullable은 일반 [[객체(Object)]]뿐만 아니라 null값까지 입력으로 받을 수 있다.


```java
@Test 
public void givenNonNull_whenCreatesNullable() { 
	String name = "saelobi"; 
	Optional<String> opt = Optional.ofNullable(name); 
	assertEquals("Optional[saelobi]", opt.toString()); 
}

@Test
public void givenNull_whenCreatesNullable() {
	String name = null; // null까지 입력 받을 수 있기 때문에 오류가 나지 않는다.
	Optional<String> opt = Optional.ofNullable(name);
	assertEquals("Optional.empty", opt.toString());
}

```