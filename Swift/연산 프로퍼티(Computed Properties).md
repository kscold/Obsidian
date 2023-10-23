
- [[클래스(class)]]와 [[구조체(struct)]]에서 사용 가능하다
- 연산 프로퍼티는 실제 값을 저장하는 [[프로퍼티(Property)]]가 아니라, `특정 상태에 따른 값을 연산하는 프로퍼티`이다. [[인스턴스(instance)]] 내/외부의 값을 연산하여 적절한 값을 돌려주는 `getter`의 역할이나 은닉화된 내부의 프로퍼티 값을 간접적으로 설정하는 `setter`의 역할을 수행할 수도 있다. 연산 프로퍼티는 클래스와 구조체 열거형에 모두 추가 가능하다.
- 변수(var)에 선언 가능하지만 상수(let)에 선언 불가능하다.

- get 메소드만 구현 가능(읽기 전용 연산 프로퍼티 가능)하지만 set 메서드만 구현 불가능하다.(즉, 쓰기 전용 연산 프로퍼티 불가능)

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