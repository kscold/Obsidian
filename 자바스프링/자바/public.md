
[[클래스(Class)]]를 선언

```java
public String hello(Model model) { 
	... 
}
```

위의 경우
"hello"라는 이름의 public [[메서드(Method)]]를 선언하고 있으며, 파라미터로 Model [[객체(Object)]]를 받고 있음

Model 객체로 인해 뒤에서 추가적인 함수로 모델에 특정 로직을 적용시킬 수 있음

이때 리턴 값은 view에 들어갈 내용이며


### 접근 범위
접근 범위는 public > protected > (default) > [[private]] 순이다.

- public은 전체적으로 접근이 가능하며,

- protected는 동일 클래스, 동일 패키지, 다른패키지의 자손클래스에서 접근이 가능, 

- (default)는 동일 클래스, 동일 패키지 내에서만 접근 가능, 

- private는 클래스 내에서만 접근이 가능하다.

접근제어자가 default라는 것은 아무런 접근 제어자도 붙이지 않는것을 의미한다.


따라서 주로 함수([[메서드(Method)]])를 public으로 선언한다.