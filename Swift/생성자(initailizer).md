- [[인스턴스(instance)]]가 생성될 때 필요한 과정을 수행한다.
- 주로, [[저장 프로퍼티(Stored Properties)]] 초기화(초기값 설정)한다.
- Init [[메소드(method)]]로써 반환값은 없다.

#### 생성자 종류
• 기본 생성자
• [[memberwise]]
• 사용자 정의 생성자

기본 생성자
• 매개변수가 없는 생성자
• [[클래스(class)]], [[구조체(struct)]], [[열거형(enum)]]은 기본 생성자를 기본적으로 제공하며 사용자가 [[재정의(override)]]하여 사용 가능

기본 생성자의 형식
``` swift
import Swift

// 1. 클래스
class SomeClass {
	init() { // 기본 생성자 재정의
		// 초기화를 위한 코드
	}
}

// 2. 구조체
struct SomeStruct {
	init() { // 기본 생성자 재정의
		// 초기화를 위한 코드
	}
}

// 3. 열거형
enum SomeEnum {
	case a, b
	init() { // 기본 생성자 재정의
	self = .a // 초기화를 위한 코드 (열거형의 인스턴스는 case중 하나의 값으로 초기화해야 함)
	}
}
```

밑에는 기본생성자,  memberwise 생성자, 사용자 정의 생성자의 예시
```swift
struct CoordinatePointA {
	var x: Int
	var y: Int
	
	init() { // 기본 생성자 재정의
		self.x = 0 // 생성자를 통한 저장 프로퍼티 초기화
		self.y = 0 // 생성자를 통한 저장 프로퍼티 초기화
	}
	
	init(x: Int, y: Int) { // memberwise 생성자 재정의
		self.x = x // 생성자를 통한 저장 프로퍼티 초기화
		self.y = y // 생성자를 통한 저장 프로퍼티 초기화
	}
	
	init(a: Int) { // 사용자 정의 생성자
		self.x = a // 생성자를 통한 저장 프로퍼티 초기화
		self.y = a // 생성자를 통한 저장 프로퍼티 초기화
	}
}

let aPoint1: CoordinatePointA = CoordinatePointA()
let aPoint2: CoordinatePointA = CoordinatePointA(x:1, y:2)
let aPoint3: CoordinatePointA = CoordinatePointA(a:0)
```


 실패 가능한 생성자
• 생성자에서 초기화에 실패한 경우 nil 반환한다.
• init 키워드 대신 init? 키워드 사용한다.
```swift
import Swift

class Person {
	let name: String
	var age: Int?
	
	init?(name: String) {
		if name.isEmpty {
			return nil
		}
			
		self.name = name
	}
	
	init?(name: String, age: Int) {
		if name.isEmpty || age < 0 {
			return nil
		}
	
		self.name = name
		self.age = age
	}
}

let kim: Person? = Person(name: "kim", age: 99)

if let person: Person = kim { // 값이 nil인지 아닌지 검사
	print(person.name)
} else {
	print("initialization fail")
}
// 실행 결과: kim
```