
선언한 [[메소드(Method)]]가 오버라이드 되었다는 것을 나타낸다.

만약 상위(부모) 클래스(또는 인터페이스)에서 해당 [[메소드(Method)]]를 찾을 수 없다면

컴파일 에러를 발생 시킨다.

[[오버라이딩(Overriding)]]을 올바르게 했는지 컴파일러가 체크한다.

Override는 오버라이딩할 때, 메서드의 이름을 잘못적는 실수를 방지해준다.

```java
class Parent{
	void parentMethod(){}
}

class Child extends Parent{
	@Override
    void pparentmethod(){} // 컴파일 에러! 잘못된 오버라이드 스펠링 틀림
```