- **프로퍼티**는 [[클래스(class)]], [[구조체(struct)]], 열거형 등에 내부에 관련된 값을 뜻한다.
- 프로퍼티에는 get, set, didSet, willSet을 사용할 수 있다.
- 그러나 하나의 프로퍼티에 [[get, set]], [[didSet, willSet]]을 모두 함께 사용하지는 못한다.

프로퍼티는 크게 5가지가 존재한다.

1. [[저장 프로퍼티(Stored Properties)]] 
2. [[지연 저장 프로퍼티(Lazy Stroed Properties)]]
3. [[연산 프로퍼티(Computed Properties)]]
4. [[타입 프로퍼티(Type Properties)]]
5. [[프로퍼티 감시자(Property Observers)]]

주로 사용되는 프로퍼티는 3가지이다.

저장 프로퍼티(stored property)
- 일반적인 프로퍼티
- 타입(클래스, 구조체)의 인스턴스에서 값을 저장하는 프로퍼티

연산 프로퍼티(computed property)
- 저장 프로퍼티처럼 값을 저장하는 프로퍼티가 아니라 저장 프로퍼티의 get 메서드와 set 메
서드를 수행 

타입 프로퍼티(type property)
- C 언어의 static 변수와 유사
- 타입(클래스, 구조체)의 인스턴스의 프로퍼티가 아니라 타입의 프로퍼티
- 타입(클래스, 구조체)의 인스턴스 없이, 타입을 통해 접근 가능한 프로퍼티








