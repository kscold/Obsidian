
- [[메서드(Method)]]로 모델 객체에 데이터를 추가할 때 사용한다.
- [[mustache]]나 [[JSP]]의 보간법인 변수에 데이터를 전달한다.

```java
public static void main(Model model) {
	model.addAttribute("data", "hello!!");
}
```

- 위의 코드는 view의 data라는 변수 이름에 "hello!!" 문자열값을 대입한다 의미이다.

이 때 중요한 것은 파라미터로 key와 value을 가지며  여기서 key는 arrtibuteName으로 value는 arrituteValue로 들어가게 된다.

즉 예시의 키의 값 data가 view 부분의 변수가 된다.