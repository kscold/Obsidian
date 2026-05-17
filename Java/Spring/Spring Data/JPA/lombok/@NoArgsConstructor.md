- 스프링에서 [[엔티티(Entity)]]를 생성했을 때 @NoArgsConstructor를 필수적으로 사용한다.
- 따라서 @NoArgsConstructor [[어노테이션(Annotation)]]을 사용하면 기본 [[생성자(constructor)]] 코드를 작성하지 않아도 된다

- 엔티티를 다루다보면 생성자를 만들어야 할 때가 생기고, 개발자의 편의를 위해 [[롬복(lombok)]] 를 자주 쓰기 마련이다.

- 보통 우리가 많이 쓰는 생성자 어노테이션은 밑의 종류가 있다.
	- [[@AllArgsConstructor]]
	- @RequiredArgsConstructor
	- @NoArgsConstructor


## 엔티티는 무조건 NoArgsConstructor를 가져야 하는 이유


```java
@Entity
@RequiredArgsConstructor
public class SomeEntity {

	@Id
	@GeneratedValue
	private Long id;
	private final String title; // 인스턴스일 때 상수 선언
}
```

- 에러 위에 마우스 커서를 두면 no-args constructor가 없어서 에러가 나오는 것(`Class 'SomeEntity' should have [public, protected] no-arg constructor`) IDE에 뜬다.

###  왜 다른 생성자가 있을 때만 이 에러가 발생할까?
-  자바는 생성자가 없으면 자동으로 No-Args-Constructor를 생성한다.

```java
public class Test {
	private String something;
}

class SubClass {
	void method() {
		Test test = new Test();
	}
}
```

- 위 예시 코드에서 보이는 것 처럼, Test 클래스는 [[생성자(constructor)]]가 없지만 [[new]] 호출이 가능하다.
- 자바에서는 생성자가 없을 때 자동으로 No Args Constructor가 생성되기 때문이다.

- 따라서 생성자가 있으면 No-Args-Constructor는 자동으로 생성되지 않는다.


```java
public class Test {
	private String something;
	
	public Test(String something) {
		this.something = something;	
	}
}

class SubClass {
	void method() {
		Test test = new Test(); // 에러 발생
	}
}
```

- new 연산자에서 에러가 발생한다.

- 따라서 엔티티에서 다른 생성자를 만들면, NoArgsConstructor를 직접 정의해야 한다.
- 다른 생성자가 있으면 NoArgsConstructor가 자동으로 생성되지 않기 때문에, 직접 정의해주지 않는 한 자동으로 생성되지 않기 때문에 에러가 발생했던 것이다.

### 3. NoArgsConstructor는 왜 필요할까

- 이유는 Proxy 패턴 때문이다.

- [[JPA(Java Persistence API)]]에서는 자주 Lazy Loading을 통해서, 객체를 프록시 형태로 조회한다.

####  프록시가 적용되는 방식

- [[엔티티(Entity)]]를 상속하는 프록시 [[객체(Object)]]를 정의한다.
- 이 프록시 객체를 초기화하기 위해 부모 객체, 즉 엔티티의 NoArgsConstructor를 호출한다.
- 그래서 NoArgsConstructor가 필요한 것이다.

#### NoArgsConstructor의 access level이 protected 이상이어야 하는 이유

- 위에서 설명한 것 처럼 자식 객체가 부모 객체의 생성자를 호출해야 하기 때문이다.
- [[private]]으로 설정된 경우, 자식 객체는 부모 객체의 생성자에 접근할 수 없기 때문에 에러가 발생한다.