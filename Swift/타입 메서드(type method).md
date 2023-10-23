- 타입([[클래스(class)]], [[구조체(struct)]])의 [[인스턴스(instance)]] 없이, 타입을 통해 호출 가능한 [[메소드(method)]]이다.

- 클래스의 타입 메소드 정의
	- static 키워드를 사용하여 메서드를 타입 메서드로 정의하면 상속 후 재정의(override) 불가능하다.
	- class 키워드를 사용하여 메서드를 타입 메서드로 정의하면 상속 후 재정의(override) 가능하다.

밑에 코드는 클래스의 타입 메소드를 재정의(override)하여 사용하는 예시
```swift
class AClass {
	static func staticTypeMethod() {
		print("AClass static type method")
	}

	class func classTypeMethod() {
		print("AClass class type method")
	}
}

class BClass: AClass {
	/*
	override static func staticTypeMethod() { 
	// 에러 발생: static 타입 메서드는 재정의 불가능
		print("overriding AClass static type method")
	}
	*/
	
	override class func classTypeMethod() { // class 타입 메서드는 재정의 가능
		print("overriding AClass class type method")
	}
}

AClass.staticTypeMethod() // 인스턴스 없이 타입(클래스)을 통해 타입 메서드 호출 가능
AClass.classTypeMethod()
BClass.classTypeMethod()

/* 
실행결과
AClass static type method
AClass class type method
overriding AClass static type method 
*/
```

- 구조체의 타입 메서드 정의
	- static 키워드를 사용하여 메소드를 타입 메소드로 정의한다.
```swift
struct SystemVolume {
	static var volume: Int = 5 // 타입 프로퍼티
	
	static func mute() { // 타입 메서드
		Self.volume = 0 // 타입 프로퍼티 접근
	}
}

class Navigation {
	var volume: Int = 5
	
	func guideWay() {
		SystemVolume.mute() // 타입 메서드 호출
	}
	
	func finishGuideWay() {
		SystemVolume.volume = self.volume // 타입 프로퍼티 접근
	}
}

SystemVolume.volume = 10
SystemVolume.mute()
var myNavi: Navigation = Navigation()

myNavi.guideWay()
print(SystemVolume.volume) // 실행 결과: 0

myNavi.finishGuideWay()
print(SystemVolume.volume) // 실행 결과: 5
```