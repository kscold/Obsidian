- self는 [[프로퍼티(Property)]]에 속한다.
-  타입([[클래스(class)]], [[구조체(struct)]])의 [[인스턴스(instance)]]가 생성되었을 때, 인스턴스 자기 자신을 나타내는 프로퍼티이다.

-  클래스의 인스턴스는 참조 타입이라서 클래스에서 self 프로퍼티 변경 불가능(인스턴스 생성시
사용자가 변수/상수로 저장한 인스턴스의 참조를 변경하기 어려움)하다.

밑에 코드는 클래스에서 self 프로퍼티 변경을 하는 코드
```swift
class LevelClass {
	var level: Int = 0
	
	func reset() {
		self = LevelClass() // 에러발생: 클래스에서 self 프로퍼티 변경 불가능
	}
}
```

- 구조체와 열거형의 인스턴스는 값 타입이라서 구조체와 열거형에서 self 프로퍼티 변경 가능하다.
밑에 코드는 구조체에서 self 프로퍼티 변경을 하는 코드([[mutaing]] 사용)
```swift
struct LevelStruct{
	var level: Int = 0
	mutating func levelUp() { 
	// 구조체에서 메서드가 프로퍼티를 변경하므로 mutating 키워드 사용
	self.level += 1
}
	mutating func reset() { // 구조체에서 메서드가 프로퍼티를 변경하므로 mutating 키워드 사용
		self = LevelStruct() // 구조체에서 self 프로퍼티 변경 가능
	}
}

var levelStructInstance: LevelStruct = LevelStruct()

levelStructInstance.levelUp()
levelStructInstance.reset()

print(levelStructInstance.level)
// 실행 결과: 0


enum OnOffSwitch {
	case on, off
	
	mutating func swapState() { 
	// 열거형에서 메서드가 프로퍼티를 변경하므로 mutating 키워드 사용
		self = self == .on ? .off : .on // 열거형에서 self 프로퍼티 변경 가능
	}
}

var toggle: OnOffSwitch = OnOffSwitch.off

toggle.swapState()

print(toggle)
// 실행 결과: on
```