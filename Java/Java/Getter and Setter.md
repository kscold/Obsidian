[[private]] [[멤버 변수(Nember Variable)]]를 다룰때 이들에게 접근하기 위해 사용하는 메소드이다.
- 클래스에 내부에 접근을 해야할 때 -> 외부 클래스에서 사용을 할 때

기본 변수를 사용하여 set할 때와 get할 때 유용하다. 

주로 [[클래스(Class)]] 내부에서만 동작하는 private와 작동된다.

  

제너레이트 키 [[인텔리제이 단축키#^5cbf53]], [[이클립스 단축키#^378158]]로 생성할 수 있는 명령어이다.


예시)
```java
static class Hello {

	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

