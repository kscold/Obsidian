
• 타입(클래스, 구조체)의 인스턴스가 생성되었을 때, 인스턴스를 통해 호출 가능한 메소드이다.

밑에 코드는 클래스의 인스턴스 메소드를 사용하는 예시
```swift
class LevelClass {
	var level: Int = 0 { // 저장 프로퍼티
		didSet { // 소멸될 때 실행
			print("Level \(self.level)")
		}
	}
	
	func levelUp() { // 인스턴스 메소드
		print("Level Up")
		self.level += 1
	}
	
	func levelDown() { // 인스턴스 메소드
		print("Level Down")
		self.level -= 1
		
		if self.level < 0 {
			self.reset()
		}
	}
	
	func jumpLevel(to: Int) { // 인스턴스 메소드
		print("Jump to \(to)")
		self.level = to
	}
	
	func reset() { // 인스턴스 메소드
		print("Reset!")
		self.level = 0
	}
}

var levelInstance: LevelClass = LevelClass() // 인스턴스 생성

levelInstance.levelUp()
/* 실행 결과
Level Up
Level 1 (didSet 메서드 호출됨)
*/

levelInstance.levelDown()
/* 실행 결과
Level Down
Level 0 (didSet 메서드 호출됨)
*/

levelInstance.levelDown()
/* 실행 결과
Level Down
Level -1 (didSet 메서드 호출됨)
Reset!
Level 0 (didSet 메서드 호출됨)
*/

levelInstance.jumpLevel(to: 3)
/* 실행 결과
Jump to 3
Level 3 (didSet 메서드 호출됨)
*/
```


- mutating 키워드: 구조체(struct)와 열거형(enum)에서 해당 [[메소드(method)]]가 [[프로퍼티(Property)]]를 변경 한다는 것을 명시

밑에 코드는 [[구조체(struct)]]의 인스턴스 메소드를 사용하는 예시
```swift
struct LevelStruct {
	var level: Int = 0 { // 저장 프로퍼티
	didSet {
		print("Level \(self.level)")
	}
}

mutating func levelUp() { // 인스턴스 메서드
// mutating을 사용함으로써 프로퍼티를 변경한다는 것을 명시
	print("Level Up")
	self.level += 1 // 인스턴스 내의 저장 프로퍼티 변경
}

mutating func levelDown() { // 인스턴스 메서드
	print("Level Down")
	self.level -= 1 // 인스턴스 내의 저장 프로퍼티 변경
	if self.level < 0 {
		self.reset()
	}
}

mutating func jumpLevel(to: Int) { // 인스턴스 메서드
	print("Jump to \(to)")
	self.level = to // 인스턴스 내의 저장 프로퍼티 변경
}

mutating func reset() { // 인스턴스 메서드
		print("Reset!")
		self.level = 0 // 인스턴스 내의 저장 프로퍼티 변경
	}
}

var levelInstance: LevelStruct = LevelStruct()
levelInstance.levelUp()
/* 실행 결과
Level Up
Level 1 (didSet 메서드 호출됨)
*/

levelInstance.levelDown()
/* 실행 결과
Level Down
Level 0 (didSet 메서드 호출됨)
*/

levelInstance.levelDown()
/* 실행 결과
Level Down
Level -1 (didSet 메서드 호출됨)
Reset!
Level 0 (didSet 메서드 호출됨)
*/

levelInstance.jumpLevel(to: 3)
/* 실행 결과
Jump to 3
Level 3 (didSet 메서드 호출됨)
*/
```

 callAsFunction 메소드
- [[인스턴스(instance)]]를 함수처럼 호출하도록 하는 [[메소드(method)]]이다.
- 다중정의(overloading) 가능하다.
- 메소드 명은 동일하지만 매개변수와 반환 타입을 다르게 해서 여러 개의 메소드를 생성 가능하다.

즉, `callAsFunction` 메서드는 함수처럼 호출될 때 어떤 일이 발생해야 하는지를 정의하는 데 사용된다. 이로써 코드를 간결하게 만들고, 인스턴스를 호출할 때 발생하는 작업을 더 명확하게 표현할 수 있다. 이러한 기능을 제공하여 코드를 읽기 쉽고 직관적으로 만들어 개발자가 인스턴스를 다루기 쉽게 한다.

```swift
struct Puppy {
	var name: String = "blacky"
	
	func callAsFunction() {
		print("\(name)")
	}

	func callAsFunction(something: String, times: Int) {
		print("\(something) \(times) times")
	}
	
	func callAsFunction(color: String) -> String {
		return "\(color) body"
	}

	mutating func callAsFunction(name: String) {
		self.name = name
	}
}

var dog: Puppy = Puppy()
dog.callAsFunction() // 실행 결과: blacky
dog() // 실행 결과: blacky
dog.callAsFunction(something: "running", times: 3) // 실행 결과: running 3 times
dog(something: "running", times: 3) // 실행 결과: running 3 times
print(dog.callAsFunction(color: "yellow")) // 실행 결과: yellow body
print(dog(color: "yellow")) // 실행 결과: yellow body
dog.callAsFunction(name: "lucy")
dog(name: "lucy")
```