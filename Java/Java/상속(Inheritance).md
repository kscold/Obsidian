- 상속이란 말 그대로 물려준다는 뜻이다. 

- 상속은 기존에 존재하는 유사한 클래스로부터 속성과 동작을 이어받고 자신이 필요한 기능을 추가하는 기법이다. 

- 상속되는 클래스를 상위 클래스(super class)([[부모 클래스(super class)]]) (=조상, 부모, 수퍼, 기반 클래스)라고 하고 상속을 받는 클래스를 하위 클래스(sub class)([[자식 클래스(sub class)]]) (=자손, 자식, 서브, 파생된 클래스)라고 한다.

- 상속의 종류는 크게 [[extends]]와 [[implements]]가 있다.


### 상속 정리
1. extends는 일반 [[클래스(Class)]]와 [[추상 클래스(Abstract Class)]] 상속에 사용되고, implement는 [[인터페이스(Interface)]] 상속에 사용된다.

2. class가 class를 상속받을 땐 extends를 사용하고, interface가 interface를 상속 받을 땐 extends를 사용한다.

3. class가 interface를 사용할 땐 implements를 써야하고 interface가 class를 사용할 땐 implements를 쓸수 없다.

4. extends는 클래스 한 개만 상속 받을 수 있다.
 
5. extends 자신 클래스는 부모 클래스의 기능을 사용한다.

6. implements는 여러개 사용 가능하고, implements는 설계 목적으로 구현 가능하다.

7. implements한 클래스는 implements의 내용을 다 사용해야 한다.