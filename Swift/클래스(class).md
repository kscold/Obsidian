- 클래스 명은 대문자 camel case를 사용한다.
- 클래스 내부의 프로퍼티와 [[메소드(method)]]는 소문자 camel case를 사용한다.
- 부모클래스를 상속받지 않고 단독으로 사용이 가능하다.
- 부모클래스를 상속받을 때 콜론(:) 뒤에 부모클래스 명을 명시한다.
- 클래스의 인스턴스는 참조(reference) 타입니다.(call by reference)
- [[소멸자(deinitailizer)]]는 클래스에만 존재한다.
- 참조 횟수 계산(reference counting)은 클래스의 인스턴스만 존재한다.

형식
```swift
class 클래스명 {
	프로퍼티와 메소드들
}

class 클래스명: 부모클래스 명{
	프로퍼티와 메소드들
}
```

#### 클래스의 [[인스턴스(instance)]] 생성 및 초기화
- 클래스의 인스턴스 생성 및 초기화시 [[생성자(initailizer)]]를 사용한다.
	-  [[구조체(struct)]]처럼 기본적으로 생성되는 [[memverwise]] 생성자가 없다.

- 클래스의 인스턴스 생성 및 초기화된 후 프로퍼티에 접근할 때는 comma(.)를 사용한다.

- 클래스의 인스턴스는 참조타입이다.(reference)
	- 구조체의 인스턴스 변수 var으로 선언하면 인스턴스 내부의 프로퍼티를 변경 가능하다.
	- 구조체의 인스턴스 상수 let으로 선언하면 인스턴스 내부의 프로퍼티를 변경 가능하다.

#### 클래스의 인스턴스 소멸
- 클래스의 인스턴스는 참조 타입으로 인스턴스 메모리에서 해제되기 직전(즉, 인스턴스가 소멸되기 직전) 처리해야 할 코드를 [[소멸자(deinitailizer)]]에 정의해 놓는다.

```swift
class Person {
	var height: Float = 0.0
	var weight: Float = 0.0
	
	init() { // 기본 생성자 재정의 -> 파라미터가 없기 때문에
		self.height = 10.0
		self.weight = 10.0
	}

	init(height: Float, weight: Float) { // 기본 memberwise 생성자
		self.height = height
		self.weight = weight
	}
	
	init(a: Float) { // 사용자 정의 memberwise 생성자
		self.height = a
		self.weight = a
	}

	deinit { // 소멸자를 재정의 한다.
		print("Person class instance is deinitialized")
	}
}

var aPerson: Person = Person()
aPerson.height = 123.4
aPerson.weight = 123.4

var bPerson: Person = Person(height: 123.4, weight: 123.4)
// memberwise 생성자를 통한 인스턴스 초기화
var cPerson: Person = Person(a: 123.4)
// 사용자 정의 생성자를 통한 인스턴스 초기화

var dPerson: Person? = Person()
dPerson = nil // 클래스 인스턴스가 메모리에서 소멸됨(소멸자 호출됨)
```