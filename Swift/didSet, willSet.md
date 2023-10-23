- 스위프트는 [[프로퍼티 감시자(Property Observers)]]로 didSet, willSet을 제공한다. 
- didSet, willSet의 역할은 [[프로퍼티(Property)]]의 값이 변경되기 직전, 직후를 감지하는 것이다.
- 따라서 이때 원하는 작업을 수행 할 수 있다. 

밑에 코드는 기본적인  didSet과 willSet의 문법을 나타낸 예시
```swift
var myProperty: Int = 10{  
	didSet(oldVal){  
	    //myProperty의 값이 변경된 직후에 호출, oldVal은 변경 전 myProperty의 값  
	}  
	willSet(newVal){  
	    //myProperty의 값이 변경되기 직전에 호출, newVal은 변경 될 새로운 값  
	}  
}
```