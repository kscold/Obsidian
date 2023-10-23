
- 가장 단순한 개념의 [[프로퍼티(Property)]]로써 [[클래스(class)]] 또는 [[구조체(struct)]]의 [[인스턴스(instance)]]와 연관된 값을 저장하는 프로퍼티이다.

- 일반적인 프로퍼티이다.
- 타입(클래스, 구조체)의 인스턴스에서 값을 저장하는 프로퍼티이다.

[[memberwise]] [[생성자(initailizer)]]
- 저장 프로퍼티 명들을 매개변수로 하는 생성자
- 구조체는 memberwise 생성자를 기본적으로 제공하여 사용자가 재정의하여 사용 가능

-  [[구조체(struct)]]의 저장 프로퍼티가 [[옵셔널(Optional)]]이 아니더라도, 구조체는 저장 프로퍼티를 모두 포함하는 [[생성자(initailizer)]]를 자동으로 생성한다.
- 하지만 [[클래스(class)]]의 저장 프로퍼티는 [[옵셔널(Optional)]]이 아니라면 프로퍼티 기본값을 지정해주거나 사용자정의 생성자통해 반드시 초기화해주어야 한다.
- 반대로 저장 프로퍼티가 [[옵셔널(Optional)]]이면 클래스, 구조체, 열거형 내에서 초기값을 설정하거나 생성자를 통해 초기화해야 할 필요가 없다.

예시 1
```swift
struct CoordinatePoint {  
  var x: Int  // 저장 프로퍼티  
  var y: Int  // 저장 프로퍼티  
}  
  
//구초제는 기본적으로 저장 프로퍼티를 매개변수로 가지는 생성자가 있기 떄문에 자동으로 생성 
let point: CoordinatePoint = CoordinatePoint(x: 10, y: 5)  
  
class Position {  
  var point: CoordinatePoint  // 저장 프로퍼티(변수)  
  
  let name: String    // 저장 프로퍼티(상수)  
  
  init(name: String, currentPoint: CoordinatePoint) {  
      self.name = name  
      self.point = currentPoint  
  }  
}  
  
// 클래스의 경우 사용자정의 이니셜라이저를 호출해야만 함  
// 그렇지 않으면 프로퍼티 초깃값을 할당할 수 없기 때문에 인스턴스생성이 불가능 
let position: Position = Position(name: "김승찬", currentPoint: point)
```

예시 2
```swift
import Swift

// 1. 구조체: memberwise 생성자가 기본적으로 제공됨
struct CoordinatePoint {
	var x: Int // 저장 프로퍼티
	var y: Int // 저장 프로퍼티
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

// 2. 클래스: memberwise 생성자를 기본적으로 제공하지 않음(사용자가 직접 정의해야 함)
class Position {
	var point: CoordinatePoint // 저장 프로퍼티
	let name: String // 저장 프로퍼티
	
	init(currentPoint: CoordinatePoint, name: String) { 
	// 사용자에 의해 정의된 memberwise 생성자
		self.name = name
		self.point = currentPoint
	}
}

let kimPosition: Position = Position(currentPoint: kimPoint, name: "park")
```

- 저장 프로퍼티가 옵셔널이 아니면 반드시 클래스, 구조체 내에서 초기값을 설정하거나 생성자를 통해 초기화를 해야한다.
- 구조체 : 기본 생성자. memberwise 생성자를 기본적으로 제공하며 사용자가 재정의할 수 있다.
- 클래스 : 기본 생성자. 소멸자를 기본적으로 제공하며 사용자가 재정의할 수 있다.

```swift
struct CoordinatePoint {
	var x: Int = 0 // 초기값이 설정된 저장 프로퍼티
	var y: Int = 0 // 초기값이 설정된 저장 프로퍼티
}

let kimPoint: CoordinatePoint = CoordinatePoint()

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

- 저장 프로퍼티가 옵셔널이면 반드시 클래스, 구조체 내에서 초기값을 설정하거나 생성자를 통해 초기화해야 할 필요가 없다.

```swift
import Swift

// 1) 구조체
struct CoordinatePoint {
	var x: Int? // 저장 프로퍼티가 옵셔널인 경우
	var y: Int? // 저장 프로퍼티가 옵셔널인 경우
}

let point: CoordinatePoint = CoordinatePoint()

// 2) 클래스
class PositionA {
	var point: CoordinatePoint? // 저장 프로퍼티가 옵셔널인 경우
	var name: String? // 저장 프로퍼티가 옵셔널인 경우
}

let aPosition: PositionA = PositionA()

class PositionB {
	var point: CoordinatePoint? // 저장 프로퍼티가 옵셔널인 경우
	var name: String = "kim" // 저장 프로퍼티가 옵셔널이 아닌 경우
}

let bPosition: PositionB = PositionB()

class PositionC {
	var point: CoordinatePoint? // 저장 프로퍼티가 옵셔널인 경우
	var name: String // 저장 프로퍼티가 옵셔널이 아닌 경우
	
	init(name: String) {
		self.name = name
	}
}

let cPosition: PositionC = PositionC(name:"kim")
```


- 클래스 또는 구조체의 [[인스턴스(instance)]]는 사용되기 전 반드시 생성자를 통해 초기화되어야 한다.
```swift
import Swift

// 1) 구조체
struct CoordinatePoint {
	var x: Int = 0 // 초기값이 설정된 저장 프로퍼티
	var y: Int = 0 // 초기값이 설정된 저장 프로퍼티
}

var kimPoint: CoordinatePoint = CoordinatePoint()

/* 또는
var kimPoint: CoordinatePoint
kimPoint = CoordinatePoint()
*/

kimPoint.x = 10
kimPoint.y = 20

/*
let kimPoint: CoordinatePoint // 에러 발생(구조체의 인스턴스는 사용되기 전 반드시 생성자를 통해 초기화되어야 함)
kimPoint.x = 10
kimPoint.y = 20
*/

// 2) 클래스
class Position {
	var point: CoordinatePoint = CoordinatePoint() // 초기값이 설정된 저장 프로퍼티
	var name: String = "unkwon" // 초기값이 설정된 저장 프로퍼티
}

let kimPosition: Position = Position()

kimPosition.point = kimPoint
kimPosition.name = "park"

/*
let kimPosition: Position // 에러 발생(클래스의 인스턴스는 사용되기 전 반드시 생성자를 통해 초기화되어야 함)
kimPosition.point = kimPoint
kimPosition.name = "park"
*/
```