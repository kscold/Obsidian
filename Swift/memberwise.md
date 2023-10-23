- [[프로퍼티(Property)]] 명들을 매개변수로 하는 [[생성자(initailizer)]]이다.

- [[구조체(struct)]]는 memberwise 생성자를 기본적으로 제공하여 사용자가 [[재정의(override)]]하여 사용 가능하다.

```swift
struct Info {
	// struct 들의 property
	var name: String
	let age: Int
	
	init (name: String, age: Int){ // memberwise 생성자
	// 위에서 정의한 프로퍼티명들을 매겨변수로 만듬
		self.name = name
		self.age = age
	}
}
```

밑에 코드는 [[구조체(struct)]]와 [[클래스(class)]]의 memberwise 생성자 사용법의 차이를 나타낸 예시
```swift
import Swift

// 1. 구조체: memberwise 생성자가 기본적으로 제공됨
struct CoordinatePoint {
	var x: Int
	var y: Int
}

/* 위의 구조체와 동일함
struct CoordinatePoint {
	var x: Int
	var y: Int
	
	init(x: Int, y: Int) { // memberwise 생성자
		self.x = x
		self.y = y
	}
}
*/

let kimPoint: CoordinatePoint = CoordinatePoint(x:10, y:5)

// 2. 클래스: memberwise 생성자가 기본적으로 제공되지 않음(사용자가 직접 정의해야 함)
class Position {
	var point: CoordinatePoint
	let name: String
	
	init(currentPoint: CoordinatePoint, name: String) { 
	// 사용자에 의해 정의된 memberwise 생성자
		self.name = name
		self.point = currentPoint
	}
}

let kimPosition: Position = Position(currentPoint: kimPoint, name: "park")
```