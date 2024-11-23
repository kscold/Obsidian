- 자바스크립트에는 마땅히 [[클래스(class)]] 내부의 값을 은닉화를 할 수 있는 방법이 없었다.

- 다행히 [[타입스크립트(TypeScript)]]에는 접근 제어자(Access modifier)인 [[public]], [[protected]], [[private]]를 지원하며, 이를 통해 외부에서 특정 [[메서드(Method)]]나 [[속성(Property)]]에 접근 범위를 지정할 수 있다.


## 접근 제어자의 종류

### [[public]]

- 기본적으로 모든 멤버는 [[public]]으로 어디서나 접근할 수 있다.
### [[protected]]

- [[클래스(class)]] 자신과 자손 [[클래스(class)]]\에서만 접근할 수 있다.
### [[private]]

- [[protected]]와 유사하게 [[클래스(class)]] 자신에서만 접근할 수 있지만, 자손 클래스에서도 접근할 수 없다.
### [[static]]

- [[타입스크립트(TypeScript)]]는 Java나 C#에서 사용하는 정적 클래스(static class) 라고 불리는 구조를 사용하지 않는다. (정적 클래스는 [[인스턴스(Instance)]]화 할 수 없다.)
- 따라서, 정적 클래스 문법을 사용하는 것은 불필요하다.
- 단지 단순 [[객체(Object)]] [[리터럴(literal)]]을 쓰는 것과 동일하다.
### abstract

- 아직 구현하지 않은 [[메서드(Method)]]와 [[속성(Property)]]에 abstract [[키워드(Keyword)]]를 사용할 수 있다. 
- abstract 멤버를 가진 [[클래스(class)]]는 반드시 [[추상 클래스(Abstract Class)]]여야 한다.

- abstract class의 역할은 abstract 멤버를 구현할 서브 클래스의 기초 클래스가되는 것이다.