
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