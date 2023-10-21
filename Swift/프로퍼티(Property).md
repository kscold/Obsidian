- **프로퍼티**는 클래스, 구조체 또는 열거형 등에 관련된 값을 뜻한다.

프로퍼티는 크게 5가지가 존재한다.

1. 저장 프로퍼티(Stored Properties)
2. 지연 저장 프로퍼티(Lazy Stroed Properties)
3. 연산 프로퍼티(Computed Properties)
4. 프로퍼티 감시자(Property Observers)
5. 타입 프로퍼티(Type Properties)

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

####  저장 프로퍼티(Stored Properties)

가장 단순한 개념의 프로퍼티로써 클래스 또는 구조체의 인스턴스와 연관된 값을 저장하는 프로퍼티이다.

- 일반적인 프로퍼티
- 타입(클래스, 구조체)의 인스턴스에서 값을 저장하는 프로퍼티

[[memverwise]] [[생성자(initailizer)]]
- 저장 프로퍼티 명들을 매개변수로 하는 생성자
- 구조체는 memberwise 생성자를 기본적으로 제공하여 사용자가 재정의하여 사용 가능

-  [[구조체(struct)]]의 저장 프로퍼티가 [[옵셔널(Optional)]]이 아니더라도, 구조체는 저장 프로퍼티를 모두 포함하는 생성자를 자동으로 생성한다.
- 하지만 [[클래스(class)]]의 저장 프로퍼티는 [[옵셔널(Optional)]]이 아니라면 프로퍼티 기본값을 지정해주거나 사용자정의 생성자통해 반드시 초기화해주어야 한다.

예시 1
```swift
struct CoordinatePoint {  
  var x: Int  // 저장 프로퍼티  
  var y: Int  // 저장 프로퍼티  
}  
  
//구초제는 기본적으로 저장 프로퍼티를 매개변수로 가지는 이니셜라이져가 있습니다.  
let point: CoordinatePoint = CoordinatePoint(x: 10, y: 5)  
  
class Position {  
  var point: CoordinatePoint  // 저장 프로퍼티(변수)  
  
  let name: String    // 저장 프로퍼티(상수)  
  
  init(name: String, currentPoint: CoordinatePoint) {  
      self.name = name  
      self.point = currentPoint  
  }  
}  
  
//사용자정의 이니셜라이저를 호출해야만 한다.  
//그렇지 않으면 프로퍼티 초깃값을 할당할 수 없기 때문에 인스턴스 생성이 불가능 하다.  
let position: Position = Position(name: "김승진", currentPoint: point)
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
#### 지연 저장 프로퍼티(Lazy Stroed Properties)

- 저장 프로퍼티의 초기화 시점을 지연시킨다.

- 키워드로는 `lazy`를 저장 프로퍼티를 지연 저장 프로퍼티로 만들고, 지연 저장 프로퍼티는 호출이 있어야 값을 초기화하게 된다.

- 상수는 인스턴스가 완전히 생성되기 전에 초기화 되어야하므로 필요할 때 값을 할당하는 지연 저장 프로퍼티에는 맞지 않는다. 따라서 `var 키워드`를 사용하여 변수로 정의한다.

- 지연 저장 프로퍼티는 클래스 또는 구조체의 인스턴스가 생성될 때 생성자에 의해 초기화되는 것이 아니라 인스턴스가 생성된 뒤 저장 프로퍼티가 사용되는 시점에 초기화된다.

- 지연 저장 프로퍼티는 주로 복잡한 클래스나 구조체를 구현할 때 많이 사용된다. 클래스 인스턴스의 저장 프로퍼티로 다른 클래스 인스턴스나 구조체 인스턴스가 할당 되어야 할 때가 있다.  

- 예를 들어 인스턴스를 초기화하면서 저장 프로퍼티로 쓰이는 인스턴스들이 한 번에 생성되거나 모든 저장 프로퍼티를 사용할 필요가 없다면 그때 지연 저장 프로퍼티를 사용하면 된다. 즉, 지연 저장 프로퍼티를 잘 사용하면 불필요한 성능 저하나 공간 낭비를 줄일 수 있다.


```swift
struct CoordinatePoint { 
	var x: Int = 0
	var y: Int = 0 
}  

class Position {  
	lazy var point: CoordinatePoint = CoordinatePoint()
	// 저장 프로퍼티 point는 클래스의 인스턴스가 생성될 때 초기화되는 것이 아니라 사용되는 시점에 초기화됨
	let name: String
	
	init(name: String) { 
		self.name = name   
	} 
}  //Position이라는 객체를 생성됬지만 아직 CoordinatePoint는 객체에 생성되지 않은 상태

let position: Position = Position(name: "lee") 

print(position.point) // 저장 프로퍼티 point가 CoordinatePoint()로 초기화되는 시점
```

point라는 CoordinatePoint객체에 lazy가 없다면 Position이라는 객체가 생성됨과 동시에 CoordinatePoint도 생성이된다.

#### 연산 프로퍼티(Computed Properties)

- 클래스와 구조체에서 사용 가능하다
- 연산 프로퍼티는 실제 값을 저장하는 프로퍼티가 아니라, `특정 상태에 따른 값을 연산하는 프로퍼티`이다. 인스턴스 내/외부의 값을 연산하여 적절한 값을 돌려주는 `getter`의 역할이나 은닉화된 내부의 프로퍼티 값을 간접적으로 설정하는 `setter`의 역할을 수행할 수도 있다. 연산 프로퍼티는 클래스와 구조체 열거형에 모두 추가 가능하다.
- 변수(var)에 선언 가능하지만 상수(let)에 선언 불가능하다.

- get 메서드만 구현 가능(읽기 전용 연산 프로퍼티 가능)하지만 set 메서드만 구현 불가능하다.(즉, 쓰기 전용 연산 프로퍼티 불가능)

- get 메서드의 매개변수는 없으며, get 메서드에서 내부 코드가 한 줄이고 반환값의 자료형이 연산 프로퍼티의 자료형과 동일하면 return 키워드 생략가능하다.

- set 메소드의 매개변수
	- 연산 프로퍼티의 자료형과 동일하므로 자료형을 넣지 않는다.
	- set 메서드를 통해 설정되는 저장 프로퍼티의 설정값이다.
	- 매개변수가 생략되면 newValue라는 매개변수가 자동 지정된다.

#### [[메소드(method)]]를 두고 연산프로퍼티를 쓰는 이유
인스턴스 외부에서 메서드를 통해 [[인스턴스(instance)]] 내부 값에 접근 하려면 메소드를 두개(getter, setter)를 구현해야한다. (기존에 다른 언어의 경우 getter, setter에 대한 메소드를 따로 만들었다.)  

스위프트에 경우 가독성이 떨어진다고 생각이 들어서 프로퍼티가 메소드 형식보다 훨씬 더 간편하고 직관적이여서 생겨난 부분으로 추측한다.

다만, 주의해야될 점은 연산 프로퍼티는 접근자인 get메서드처럼 읽기 전용을 구현하기 쉽지만, 쓰기 전용은 구현할 수 없다는 단점이 있다.(메소드로는 가능하지만 프로퍼티로는 불가능하다.)

예시 1 구조체에서 사용자 정의 get 메서드와 set 메서드사용하는 코드
```swift
struct CoordinatePoint {  
	var x: Int
	var y: Int // 대칭점을 구하는 메서드 - 접근자(getter) 
	
	func oppsitePoint() -> CoordinatePoint { 
		return CoordinatePoint(x: -x, y: -y)
	}
	
	mutating func setOppositePoint(_ opposite: CoordinatePoint) {
		x = -opposite.x 
		y = -opposite.y 
	} 
} 

var position: CoordinatePoint = CoordinatePoint(x: 10, y: 20)  

print(position) //현재 좌표 

print(position.oppsitePoint()) //대칭 좌표   

position.setOppositePoint(CoordinatePoint(x: 15, y: 10)) 
//대칭 좌표를 (15, 10)으로 설정  

print(position) // -15, -10
```

예시 2 위에랑 거의 비슷한 코드
```swift
import Swift

struct CoordinatePoint {
	var x: Int
	var y: Int
	
	// Self(타입 자기 자신을 나타내는 프로퍼티)를 CoordinatePoint로 대체 가능
	func getOppositePoint() -> Self { // get 메소드
		return CoordinatePoint(x: -x, y: -y)
	}
	
	// mutating 키워드: 구조체와 열거형에서 메소드가 프로퍼티를 변경한다는 것을 명시하기 위해 사용
	mutating func setOppositePoint(_ opposite: CoordinatePoint) { // set 메소드
		self.x = -opposite.x
		self.y = -opposite.y
	}
}

var aPosition: CoordinatePoint = CoordinatePoint(x:10, y: 20)
print(aPosition)
// 실행 결과: CoordinatePoint(x: 10, y: 20)

print(aPosition.getOppositePoint())
// 실행 결과: CoordinatePoint(x: -10, y: -20)

aPosition.setOppositePoint(CoordinatePoint(x: 15, y: 10))
print(aPosition)
// 실행 결과: CoordinatePoint(x: -15, y: -10)
```

위의 코드는 getOppsitePoint() 메소드로 대칭점을 구할 수 있으며 setOppsitePoint() 메소드로 대칭점을 설정 해줘야 하는 코드이다.

이 코드에서 접근자와 설정자 이름의 일관성을 유지하기도 힘들며 일관되지 않아 코드를 한 번에 읽기가 쉽지 않다.  

하지만 연산 프로퍼티를 사용하면 이 두 메소드를 좀 더 간결하고 확실하게 표현할 수 있다.

밑에 예시는 구조체에서  연산 프로퍼티로 구현된 get 메소드와 set 메소드를 사용하는 코드

```swift
import Swift

struct CoordinatePoint {
	var x: Int // 저장 프로퍼티
	var y: Int // 저장 프로퍼티
	var oppositePoint: CoordinatePoint { // 연산 프로퍼티
		
		get { // get 메소드
			return CoordinatePoint(x: -x, y: -y)
		// 위의 코드는 CoordinatePoint(x: -x, y: -y)로 대체 가능
		}
		
		set(opposite) { // set 메소드
			x = -opposite.x // opposite의 자료형: CoordinatePoint
			y = -opposite.y
		}
	}
}

var aPosition: CoordinatePoint = CoordinatePoint(x: 10, y: 20)

print(aPosition)
// 실행 결과: CoordinatePoint(x: 10, y: 20)

print(aPosition.oppositePoint) // get 메서드 호출됨
// 실행 결과: CoordinatePoint(x: -10, y: -20)

aPosition.oppositePoint = CoordinatePoint(x: 15, y: 10) // set 메서드 호출됨

print(aPosition)
// 실행 결과: CoordinatePoint(x: -15, y: -10)
```

위의 코드와 같이 연산 프로퍼티를 사용하면 하나의 프로퍼티에 접근자와 설정자가 모두 모여있고, 해당 프로퍼티가 어떤 역할을 하는지 좀 더 명확하게 표현이 가능하다.
인스턴스를 사용하는 입장에서도 마치 저장 프로퍼티인 것처럼 편하게 사용할 수있다.

설정자의 매개변수로 원하는 이름을 소괄호 안에 명시해주면 set메서드 내부에서 전달받은 전달 인자를 사용할 수 있다. 관용적인 표현으로 `newValue`로 매개변수 이름을 대신할 수 있다.

읽기 전용으로 연산 프로퍼티를 사용할 수도 있다.

밑에 예시는 구조체에서  연산 프로퍼티로 구현된 get 메소드와 set 메소드를와 newValue를 사용하는 코드
 ```swift
import Swift

struct CoordinatePoint {
	var x: Int // 저장 프로퍼티
	var y: Int // 저장 프로퍼티
	
	var oppositePoint: CoordinatePoint { // 연산 프로퍼티
		get { // get 메서드
			return CoordinatePoint(x: -x, y: -y)
		// 위의 코드는 CoordinatePoint(x: -x, y: -y)로 대체 가능
		}
		set { // set 메서드
			x = -newValue.x // newValue의 자료형: CoordinatePoint
			y = -newValue.y
		}
	}
}

var aPosition: CoordinatePoint = CoordinatePoint(x: 10, y: 20)
print(aPosition)
// 실행 결과: CoordinatePoint(x: 10, y: 20)

print(aPosition.oppositePoint) // get 메서드 호출됨
// 실행 결과: CoordinatePoint(x: -10, y: -20)

aPosition.oppositePoint = CoordinatePoint(x: 15, y: 10) // set 메서드 호출됨
print(aPosition)
// 실행 결과: CoordinatePoint(x: -15, y: -10)
```

#### 타입 프로퍼티(Type Properties)

- 각각의 인스턴스가 아닌 타입 자체에 속하게 되는 프로퍼티를 `타입 프로퍼티`라고 한다.

- 타입(클래스, 구조체)의 인스턴스의 프로퍼티가 아니라 타입의 프로퍼티이다.
- 타입의 인스턴스 없이, 타입을 통해 접근 가능한 프로퍼티이다.
-  static 키워드를 사용하여 저장 프로퍼티와 연산 프로퍼티를 타입 프로퍼티로 만든다.
- 저장 타입 프로퍼티: 반드시 초기값이 설정되어 있어야 하여 기본적으로 지연 연산 되므로 lazy 키워드를 붙일 필요 없다.
- 연산 타입 프로퍼티이다.
- 동일한 타입의 인스턴스들이 타입 프로퍼티를 공유한다.

이제까지 알아본 프로퍼티 개념은 모두 타입을 정의하고 그 타입의 인스턴스가 생성되었을때 사용될 수 있는 인스턴스 프로퍼티이다. 인스턴스 프로퍼티는 인스턴스를 새로 생성할 때 마다 초깃값에 해당하는 값이 프로퍼티의 값이 되고, 각 인스턴스마다 다른 값을 지닐 수 있다.

- 타입 프로퍼티는 타입 자체에 영향을 미치는 프로퍼티이고 인스턴스의 생성 여부와 상관없이 타입 프로퍼티의 값은 하나이다.  
- 타입프로퍼티는 그 타입의 모든 인스턴스가 공통으로 사용되는 값(C 언어의 static constant와 유사)이라는지, 모든 인스턴스에서 공용으로 접근하고 값을 변경할 수 있는 변수(C 언어의 static 변수와 유사)등을 정의할 때 유용하다.

```swift
class Account {
	static var storedTypeProperty: Int = 0 // 저장 타입 프로퍼티
	
	var storedProperty: Int = 0 { // 저장 프로퍼티
		didSet {
			// Self.storedTypeProperty와 Account.storedTypeProperty는 같은 표현
			Self.storedTypeProperty = self.storedProperty + 100 
			// Self: 타입 자기 자신을 나타내는 프로퍼티
			// self: 인스턴스 자기 자신을 나타내는 프로퍼티
			// Account.storedTypeProperty = didSet.storedProperty + 100 과 같음
		}
	}
	
	static var computedTypeProperty: Int { // 연산 타입 프로퍼티
		get {
			return Self.storedTypeProperty
		}
		set {
			Self.storedTypeProperty = newValue
		}
	}
}

Account.storedTypeProperty = 123 // 인스턴스 생성없이 타입을 통해 저장 타입 프로퍼티 접근
Print(Account.computedTypeProperty) // 인스턴스 생성없이 타입을 통해 연산 타입 프로퍼티 접근
// 실행 결과: 123

let aInstance: Account = Account()
let bInstance: Account = Account()

aInstance.storedProperty = 100
bInstance.storedProperty = 200 // 같은 타입의 인스턴스들끼리 저장 타입 프로퍼티를 공유함

print(Account.storedTypeProperty) // 타입을 통해 저장 타입 프로퍼티 접근
print(Account.computedTypeProperty) // 타입을 통해 저장 타입 프로퍼티 접근

/* 실행 결과
300
300
*/
```

```swift
class Aclass {
	static var typeProperty: Int = 0  // 저장 타입 프로퍼티
	
	var instanceProperty: Int = 0 {  // 저장 프로퍼티 
	    didSet { 
			Aclass.typeProperty = instanceProperty + 100 
		}  
	} 

	static var typeComputedProperty: Int { // 연산 타입 프로퍼티 
		get {  
			return typeProperty 
		}
		
		set { 
			typeProperty = newValue 
		}  
	} 
} 

Aclass.typeProperty = 123  
let classInstance: Aclass = Aclass()  
classInstance.instanceProperty = 100 // 여기에서 typeProperty의 값이 200으로 바뀜

print(Aclass.typeProperty)  // 200  
print(Aclass.typeComputedProperty)  // 200
```

#### 프로퍼티 감시자(Property Observers)

- 감시자 프로퍼티를 사용하면 프로퍼티의 값이 변경됨에 따라 적절한 액션을 취할 수 있다. 프로퍼티의 값이 새로 할당될 때마다 호출되고 변경되는 값이 현재의 값과 같더라도 호출된다.

-  저장 프로퍼티, 연산 프로퍼티, 타입 프로퍼티는 프로퍼티 감시자(willSet 메소드와 didSet 메소드)로 구현 가능하다.
- 따라서 프로퍼티 감시자는 지연 저장 프로퍼티에는 사용할 수 없다.

- 프로퍼티 감시자에는 프로퍼티의 값이 변경되지 직전에 호출되는 `willSet메소드`와 프로퍼티의 값이 변경된 직후에 호출되는 `didSet메소드`가 있다. 

- 저장 프로퍼티가 초기화될 때 프로퍼티 감시자는 호출되지 않는다.
- 연산 프로퍼티는 클래스를 상속받았을 때만 연산 프로퍼티를 재정의하여 프로퍼티 감시자 구현이 가능하다.

- willSet 메서드의 매개변수
	- 저장 프로퍼티가 변경될 값
	- 매개변수가 생략되면 newValue라는 매개변수가 자동 지정됨
	- 
- didSet 메서드의 매개변수
	- 저장 프로퍼티가 변경되기 전의 값
	- 매개변수가 생략되면 oldValue라는 매개변수가 자동 지정됨
	

- willSet메서드에서 전달되는 전달인자는 프로퍼티가 `변경될 값`이고, didSet메서드에서 전달되는 프로퍼티의 값은 `변경되기 전의 값`이다. 
- willSet, didSet의 메소드는 매개변수가 하나씩 있는데 매개변수를 따로 지정하지 않으면 `willSet메서드에는 newValue`, `didSet메서드에는 oldValue`가 매개변수로 자동으로 저장된다.

|   |   |
|---|---|
|프로퍼티 감시자 |저장 프로퍼티 |연산 프로퍼티|
|willSet 메서드 저장 프로퍼티가 변경되기 직전 호출됨| set 메서드가 실행되기 전에 호출됨|
|didSet 메서드 저장 프로퍼티가 변경된 직후 호출됨 |set 메서드가 실행된 후에 호출됨|

밑에 코드는 저장 프로퍼티에 프로퍼티 감시자 구현한 예시
```swift
import Swift

class Account {
	var credit: Int = 0 { // 저장 프로퍼티
		willSet(newCredit) { // willSet 메서드: 저장 프로퍼티가 변경되기 직전 호출됨
			print("잔액이 \(credit)원에서 \(newCredit)원으로 변경될 예정입니다.")
		}
		didSet(oldCredit) { // didSet 메서드: 저장 프로퍼티가 변경된 직후 호출됨
			print("잔액이 \(oldCredit)원에서 \(credit)원으로 변경되었습니다.")
		}
	}
}

let myAccount: Account = Account()
// 잔액이 0원에서 10000원으로 변경될 예정입니다.
myAccount.credit = 10000
// 잔액이 0원에서 10000원으로 변경되었습니다.
```


밑에 코드는 저장 프로퍼티에 프로퍼티 감시자 구현 (willSet 메서드와 didSet 메서드의 매개변수가 없는 경우)
```swift
// 저장 프로퍼티에 프로퍼티 감시자 사용(willSet 메서드와 didSet 메서드의 매개변수가 없는 경우)
import Swift

class Account {
		var credit: Int = 0 { // 저장 프로퍼티
		willSet { // willSet 메서드: 저장 프로퍼티가 변경되기 직전 호출됨
			print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
		}
		didSet { // didSet 메서드: 저장 프로퍼티가 변경된 직후 호출됨
			print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
		}
	}
}

let myAccount: Account = Account()
// 잔액이 0원에서 10000원으로 변경될 예정입니다.
myAccount.credit = 10000
// 잔액이 0원에서 10000원으로 변경되었습니다.
```


밑에 코드는 연산 프로퍼티에 프로퍼티 감시자 구현한 예시
```swift
import Swift

class Account {
	var credit: Int = 0 { // 저장 프로퍼티
	
	willSet { // willSet 메서드: 저장 프로퍼티가 변경되기 직전 호출됨
		print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
	}
	
	didSet { // didSet 메서드: 저장 프로퍼티가 변경된 직후 호출됨
		print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
	}
}

var dollarValue: Double { // 연산 프로퍼티
		get {
			return Double(credit / 1000)
		}
		
		set {
			credit = Int(newValue * 1000)
			print("잔액을 \(newValue)달러로 변경 중입니다.")
		}
	}
}

class ForeignAccount: Account {
	override var dollarValue: Double { // 연산 프로퍼티를 재정의하여 프로퍼티 감시자 구현
		
		willSet { // willSet 메서드: set 메서드가 실행되기 전에 호출됨
			print("잔액이 \(self.dollarValue)달러에서 \(newValue)달러로 변경될 예정입니다.")
		}
		
		didSet { // didSet 메서드: set 메서드가 실행된 후에 호출됨
			print("잔액이 \(oldValue)달러에서 \(self.dollarValue)달러로 변경되었습니다.")
		}
	}
}

let myAccount: ForeignAccount = ForeignAccount()
// 잔액이 0원에서 1000원으로 변경될 예정입니다. (저장 프로퍼티의 willSet 메서드가 호출됨)
myAccount.credit = 1000
// 잔액이 0원에서 1000원으로 변경되었습니다. (저장 프로퍼티의 didSet 메서드가 호출됨)

// 잔액이 1.0달러에서 2.0달러로 변경될 예정입니다. (연산 프로퍼티의 willSet 메서드가 호출됨)
// 잔액이 1000원에서 2000원으로 변경될 예정입니다. (저장 프로퍼티의 willSet 메서드가 호출됨)
// 잔액이 1000원에서 2000원으로 변경되었습니다. (저장 프로퍼티의 didSet 메서드가 호출됨)
myAccount.dollarValue = 2 // 잔액을 2.0달러로 변경 중입니다. (연산 프로퍼티의 set 메서드가 호출됨)
// 잔액이 1.0달러에서 2.0달러로 변경되었습니다. (연산 프로퍼티의 didSet 메서드가 호출됨)

```
