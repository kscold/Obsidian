- 객체를 [[Optional]] [[객체(Object)]]로 감싸기 위해서는 Optional 에서 제공하는 [[Optional.of()]]는 인자로서 null값을 받지 않는다는 것을 나타낸다.
- 일반 객체 뿐만아니라 null 값까지 입력받을려면 [[Optional.ofNullable()]] [[메서드(Method)]]를 사용하면 된다.

```java
@Test
public void givenNonNull_whenCreatesNonNullable() {
	String name = "saelobi";
	Optional<String> opt = Optional.of(name);
	assertEquals("Optional[saelobi]", opt.toString()); // saelobi 대입
}

@Test(expected = NullPointerException.class)
public void givenNull_whenThrowsErrorOnCreate() { 
	String name = null; // of 메서드를 사용했기 때문에 null값을 메서드 입력으로 받을 시에 NullPointerException을 일으킨다.
	Optional<String> opt = Optional.of(name);
}

```