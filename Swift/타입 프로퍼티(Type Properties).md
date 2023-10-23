
- 각각의 [[인스턴스(instance)]]가 아닌 타입 자체에 속하게 되는 [[프로퍼티(Property)]]를 `타입 프로퍼티`라고 한다.


- 타입([[클래스(class)]], [[구조체(struct)]])의 인스턴스의 프로퍼티가 아니라 타입의 프로퍼티이다.
- 따라서 인스턴스로 접근이 불가능 하고 직접 클래스, 구조체 명으로 접근해야 한다.
- 타입의 인스턴스 없이, 타입을 통해 접근 가능한 프로퍼티이다.
- static 키워드를 사용하여 저장 프로퍼티와 연산 프로퍼티를 타입 프로퍼티로 만든다.
- 저장 타입 프로퍼티: 반드시 초기값이 설정되어 있어야 하여 기본적으로 지연 연산 되므로 lazy 키워드를 붙일 필요 없다.
- 연산 타입 프로퍼티이다.
- 동일한 타입의 인스턴스들이 타입 프로퍼티를 공유한다.

이제까지의 프로퍼티([[저장 프로퍼티(Stored Properties)]], [[지연 저장 프로퍼티(Lazy Stroed Properties)]], [[연산 프로퍼티(Computed Properties)]]) 개념은 모두 타입을 정의하고 그 타입의 인스턴스가 생성되었을때 사용될 수 있는 인스턴스 프로퍼티이다. 인스턴스 프로퍼티는 인스턴스를 새로 생성할 때 마다 초깃값에 해당하는 값이 프로퍼티의 값이 되고, 각 인스턴스마다 다른 값을 지닐 수 있다.

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
			// Account.storedTypeProperty = Account.storedProperty + 100 과 같음
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